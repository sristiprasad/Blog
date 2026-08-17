# The Working Prototype and What's Next

Building the prototype was one thing. Getting it to behave like a real product was another.

The first working version proved that the core idea was possible: I could send a grocery list on WhatsApp, have an AI turn it into structured items, search across multiple stores, compare prices, and return a best-price total. The user could then confirm and receive a payment link. The actual implementation connects WhatsApp through Twilio, FastAPI as the backend, an LLM for parsing, a pricing engine for comparison, MongoDB for session state, and Razorpay for payment links.

But getting a prototype to work also made the gaps much more obvious.

## Part 1: The Prototype Worked. Then Reality Happened.

The biggest surprise was that the hardest problem wasn't the AI.

Parsing a message like “2kg onions, 1L milk and 6 eggs” into structured data was relatively straightforward. The real challenge was getting reliable, real-time prices from quick-commerce platforms.

I initially tried web search. Then page scraping. Both worked just enough to make the prototype look promising — and failed enough to show why production would be much harder.

Platforms like Blinkit, Zepto and Instamart rely heavily on JavaScript-rendered pages, so the price isn't always present in the static HTML that a simple scraper can read. Even search snippets could return irrelevant numbers that looked like prices.

That was an important product lesson for me:

### <i> A working demo proves the workflow. It doesn't necessarily prove the underlying data can support the product</i>

The next technical direction is therefore less about adding more AI and more about building a reliable data layer — potentially using a headless browser such as Playwright or, better, identifying the underlying APIs used by these platforms.

## Part 2: The Cheapest Item Isn't Always the Cheapest Basket

Another assumption the prototype challenged was the definition of “best price.”

Initially, it felt obvious: find the cheapest price for every item across stores and add them up. But that creates a very different user experience. If onions are cheapest on Blinkit, milk on Zepto and eggs on Instamart, the user hasn't really saved time. They now have three orders to place.

So the problem isn't actually:

<i> “Where is each item cheapest?” </i>

It is:

<i> “What is the cheapest practical way to complete my entire grocery order?” </i>

That changes the optimisation problem completely. The next version therefore needs to think beyond item-level price comparison and consider the total basket cost, number of stores, delivery fees, availability and potentially delivery time. This was probably the most interesting product insight I got from building the prototype. Sometimes the technically optimal solution isn't the product-optimal solution.

## Part 3: From Prototype to Product

The current prototype can take a user from grocery list to payment link. But there is still a very important gap in between:

<b> What happens after the payment? </b>

Right now, Razorpay can collect the estimated amount, but the actual grocery orders aren't automatically placed on Blinkit, Zepto or the other platforms. That would require official partner APIs or browser automation, both of which introduce significant feasibility and reliability considerations.

And that is where I think the prototype becomes more interesting.

The next version isn't simply about making the AI smarter.

It needs to become more reliable, more transparent and more autonomous.

Things like:

> Re-validating prices before payment
Handling unavailable items with substitutions
Understanding follow-up messages like “remove the eggs”
Supporting Hindi and mixed-language inputs properly
Asking for clarification instead of guessing vague quantities
Handling interrupted or abandoned sessions
Optimising the entire basket instead of individual items
> Eventually completing the actual order


Some of these are engineering problems. Some are UX problems. Some are business and platform-integration problems. And that, perhaps, is my biggest takeaway from building this.

Agentic AI isn't just about getting an AI model to perform a task. It's about designing the entire system around what the AI should do, what deterministic software should handle, where failures can happen, and how the user recovers from them. 

The prototype gave me the proof of concept. Now comes the much harder question:
Can I turn the proof of concept into something a person would actually trust with their grocery order?
----
**Series** : [Building an Agentic AI Grocery Assistant: Comparing Quick Commerce Prices Through WhatsApp ->](https://github.com/sristiprasad/Blog/blob/main/series/Grocery%20Comparator%20Agent.md)

# Related

<ul>
  <li><a href="https://github.com/sristiprasad/Blog/blob/main/Posts/The%20Problem%2C%20Opportunity%2C%20and%20Initial%20Concept.md">The Problem, Opportunity, and Initial Concept:</a>Why grocery price comparison is broken and how Agentic AI can solve it.</li>
  <li><a href="https://github.com/sristiprasad/Blog/blob/main/Posts/Designing%20the%20Agent%3A%20Workflows%2C%20Edge%20Cases%2C%20and%20Technical%20Challenges.md">Designing the Agent: Workflows, Edge Cases, and Technical Challenges:</a>Architecture, tools, APIs, failures, and lessons learned while building the system.</li>