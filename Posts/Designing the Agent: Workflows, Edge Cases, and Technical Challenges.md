# Designing the Agent: Workflows, Edge Cases, and Technical Challenges


## The Core Workflow

The agent is a linear pipeline. Each step hands off to the next, and failure at any step needs a graceful fallback.

```
WhatsApp message
      ↓
Twilio receives it → POST to FastAPI webhook
      ↓
Groq LLaMA parses natural language → structured items
      ↓
Pricing engine searches each item across stores
      ↓
Confirmation message sent back via WhatsApp
      ↓
User replies YES
      ↓
MongoDB retrieves session → Razorpay link generated
      ↓
Payment link sent via WhatsApp
```

Every arrow in that diagram is a potential failure point. That's the honest version of the architecture.

---

## Step-by-Step Design Decisions

### 1. Natural Language Parsing

**The problem:** Humans describe groceries inconsistently.

These all mean the same thing:
- "2kg onions"
- "two kilos of onion"
- "onions x2 kg"
- "I need some onions, maybe 2 kilos"
- "pyaaz 2 kilo"

**The decision:** Hand this entirely to an LLM. Don't try to regex your way through human language. Claude/Groq receives the raw message and returns a clean JSON array:

```json
[
  {"name": "onions", "qty": 2, "unit": "kg"},
  {"name": "milk", "qty": 1, "unit": "L"},
  {"name": "eggs", "qty": 6, "unit": "pieces"}
]
```

**Edge cases we hit:**
- LLM returning markdown-wrapped JSON (` ```json `) instead of raw JSON — solved with a strip function
- Model getting decommissioned mid-build (llama3-8b-8192) — solved by switching to llama-3.3-70b-versatile
- Hindi/mixed language inputs — partially handled, needs more testing
- Vague quantities ("some bread", "a few eggs") — LLM makes a reasonable guess, but needs user confirmation ideally

---

### 2. Price Discovery

This turned out to be the hardest problem in the entire project. Three approaches tried, each with real limitations:

**Attempt 1 — Mock data**
Hardcoded prices per store per item. Works perfectly in a demo, useless in production. Prices change daily.

**Attempt 2 — DuckDuckGo web search**
Searched for each item and filtered results by store domain. Problems:
- Returned Wikipedia articles and Healthline blog posts
- Even with `site:` filtering, returned category pages and city pages instead of product pages
- Price extraction from snippets picked up irrelevant numbers ("2178 products available" parsed as ₹2178 for eggs)

**Attempt 3 — BeautifulSoup page scraping**
Fetched actual product pages and extracted prices from HTML. Problems:
- Blinkit, Zepto, and Instamart are React SPAs
- Prices are injected by JavaScript after page load
- BeautifulSoup only reads static HTML — sees an empty shell

**Where we are now:**
Playwright (headless browser) is the right next step. It renders JavaScript fully, meaning it can see actual prices on React apps. The tradeoff is speed — 2-3 seconds per item per store.

**The deeper architectural question:**
Should the agent find the single cheapest item across all stores, or the cheapest total basket from one store? Splitting across stores creates a logistics problem — the user needs to check out on 3 different apps. A single-store optimisation is more practical even if individual items cost slightly more.

---

### 3. Session State

**The problem:** A conversation spans multiple HTTP requests.

When the bot sends "Reply YES to pay", the server has already finished handling that request and forgotten everything. When YES arrives minutes later, the server needs to remember what was ordered.

**The decision:** Store pending orders in MongoDB keyed by phone number.

```python
# On confirmation message sent:
save_session("whatsapp:+91XXXXXXXXXX", {
    "items": priced,
    "total": 642
})

# On YES received:
session = get_session("whatsapp:+91XXXXXXXXXX")
create_payment_link(session["items"], session["total"])
clear_session("whatsapp:+91XXXXXXXXXX")
```

**Edge cases:**
- User sends a new grocery list before replying YES to the old one — session gets overwritten, old order lost
- User never replies — session sits in MongoDB forever, needs a TTL (time-to-live) index to auto-expire
- User replies "yes" with lowercase, "YES!", "Yeah sure" — currently only exact "YES" match works, needs fuzzy matching
- Two messages arrive simultaneously (unlikely but possible) — no locking mechanism currently

**Simpler alternative for MVP:** An in-memory Python dictionary. Resets on server restart but removes the MongoDB dependency entirely for single-user testing.

---

### 4. Payment

**The decision:** Razorpay payment link API. One link covers the full estimated total regardless of which stores items came from.

**The honest limitation:** This is a collected payment, not an actual order placement. The flow is:

```
User pays ₹642 via Razorpay
      ↓
??? 
      ↓
Groceries arrive
```

The middle step — actually placing orders on Blinkit/Zepto/JioMart — isn't automated. That would require either:
- Official partner API access (not publicly available)
- Selenium/Playwright automation of the checkout flow (fragile, against ToS)
- The user manually completing checkout on each store

For a real product this is the critical gap. For a learning project it demonstrates the full agentic concept.

---

## Edge Cases Map

| Scenario | Current behaviour | Ideal behaviour |
|---|---|---|
| Hindi/mixed language input | Partially works | Full multilingual support |
| Vague quantity ("some eggs") | LLM guesses | Ask user to clarify |
| Item completely unavailable | Shows as ❌ | Suggest substitute |
| User edits order after summary | Ignored | Re-process updated list |
| User says "yes" informally | Not recognised | Fuzzy match |
| Network timeout on price search | Silent failure | Retry with fallback |
| Same user, two simultaneous messages | Race condition | Queue requests |
| Session never confirmed | Stays in DB forever | TTL expiry |
| Price changes between search and payment | Stale price shown | Re-validate on YES |
| React app prices (Blinkit/Zepto) | Not extracted | Playwright headless browser |

---

## The Architectural Insight

The most important design decision in the whole project wasn't a technical one.

**Claude/Groq does exactly one job:** turn messy human text into clean structured data.

Everything after that — searching, comparing, storing, paying — is deterministic Python. No AI involved. This separation is what makes the system debuggable. When something breaks, you know immediately whether it's the AI layer or the backend layer.

That's the right way to think about agentic systems. Not "how do I make AI do everything" but "what is the smallest slice of the problem that actually needs AI, and what can deterministic code handle better?"

In this case: one function, one prompt, one JSON array out. The rest is engineering.

---

## What's Left to Solve

The two unsolved problems that matter most:

**1. Real-time price extraction from JavaScript-rendered store pages**
Playwright with endpoint discovery is the current path forward — intercept the XHR calls the app makes to its own backend, extract prices from those API responses directly rather than scraping the DOM.

**2. Actual order placement**
Without an official API, this remains a manual step. The agent can get you to the checkout page — it can't complete the checkout for you.

Everything else is polish. These two are the core product gaps.

------
**Series** : [Building an Agentic AI Grocery Assistant: Comparing Quick Commerce Prices Through WhatsApp ->](https://github.com/sristiprasad/Blog/blob/main/series/Grocery%20Comparator%20Agent.md)

# Related

<ul>
  <li><a href="https://github.com/sristiprasad/Blog/blob/main/Posts/The%20Problem%2C%20Opportunity%2C%20and%20Initial%20Concept.md">The Problem, Opportunity, and Initial Concept:</a>Why grocery price comparison is broken and how Agentic AI can solve it.</li>
  <li><a href="https://github.com/sristiprasad/Blog/blob/main/Posts/The%20Working%20Prototype%20and%20What's%20Next.md">The Working Prototype and What's Next:</a>Demo, results, limitations, and future improvements toward fully autonomous ordering.</li>