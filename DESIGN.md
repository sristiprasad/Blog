---
name: Khel Tic Tac Design System
colors:
  surface: '#00180b'
  surface-dim: '#00180b'
  surface-bright: '#004228'
  surface-container-lowest: '#001208'
  surface-container-low: '#002111'
  surface-container: '#002515'
  surface-container-high: '#00311c'
  surface-container-highest: '#003d25'
  on-surface: '#a8f3c6'
  on-surface-variant: '#d0c5af'
  inverse-surface: '#a8f3c6'
  inverse-on-surface: '#003921'
  outline: '#99907c'
  outline-variant: '#4d4635'
  surface-tint: '#e9c349'
  primary: '#f2ca50'
  on-primary: '#3c2f00'
  primary-container: '#d4af37'
  on-primary-container: '#554300'
  inverse-primary: '#735c00'
  secondary: '#ecc246'
  on-secondary: '#3d2e00'
  secondary-container: '#b18c09'
  on-secondary-container: '#352800'
  tertiary: '#f3ca4d'
  on-tertiary: '#3d2f00'
  tertiary-container: '#d6af33'
  on-tertiary-container: '#564300'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#ffe088'
  primary-fixed-dim: '#e9c349'
  on-primary-fixed: '#241a00'
  on-primary-fixed-variant: '#574500'
  secondary-fixed: '#ffe08e'
  secondary-fixed-dim: '#ecc246'
  on-secondary-fixed: '#241a00'
  on-secondary-fixed-variant: '#584400'
  tertiary-fixed: '#ffe08b'
  tertiary-fixed-dim: '#ebc246'
  on-tertiary-fixed: '#241a00'
  on-tertiary-fixed-variant: '#584400'
  background: '#00180b'
  on-background: '#a8f3c6'
  surface-variant: '#003d25'
typography:
  display-lg:
    fontFamily: Noto Serif
    fontSize: 40px
    fontWeight: '700'
    lineHeight: '1.2'
    letterSpacing: 0.15em
  headline-md:
    fontFamily: Noto Serif
    fontSize: 24px
    fontWeight: '600'
    lineHeight: '1.4'
    letterSpacing: 0.1em
  title-sm:
    fontFamily: Noto Serif
    fontSize: 18px
    fontWeight: '500'
    lineHeight: '1.5'
    letterSpacing: 0.05em
  body-md:
    fontFamily: Be Vietnam Pro
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.6'
    letterSpacing: 0.02em
  label-caps:
    fontFamily: Be Vietnam Pro
    fontSize: 12px
    fontWeight: '700'
    lineHeight: '1.0'
    letterSpacing: 0.2em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  base: 8px
  safe-margin: 24px
  grid-gutter: 12px
  token-padding: 16px
---

## Brand & Style

The design system is anchored in the concept of "Ancient Prestige." It reimagines the classic Tic Tac Toe game as a high-stakes duel of wits set within a regal, culturally-rich environment. The brand personality is intellectual, ceremonial, and sophisticated, targeting players who appreciate strategic depth and aesthetic refinement.

The visual style employs a **Minimalist-Tactile** hybrid approach. It utilizes the expansive, deep colors of traditional luxury textiles and gemstones, paired with the precision of modern geometric line art. The UI avoids cluttered ornamentation, favoring breathing room and sharp, intentional strokes that make the digital experience feel like interacting with a physical, gold-inlaid artifact.

## Colors

The palette is a focused study on the contrast between deep earth tones and precious metals. 

- **Background:** A singular, immersive Deep Emerald Green (#0B5D3B) serves as the canvas, providing a sense of stability and history.
- **Primary (Gold):** Used for the most critical interactive elements, icons, and primary titles. It represents the "active" energy of the player.
- **Secondary (Muted Gold):** Reserved for structural components like grid lines. This should be applied with an opacity range of 20% to 40% to ensure it defines the space without competing with the gameplay tokens.
- **Accent (Bright Gold):** A high-vibrancy gold used exclusively for "Winning Strike" lines and celebration states, creating a clear visual climax at the end of a round.

## Typography

This design system uses a high-contrast typographic pairing to balance tradition and mobile utility.

**Noto Serif** is the cornerstone of the identity, used for all headlines and game titles. To achieve the "Cinzel-like" elegance, titles must be set with **wide tracking (letter-spacing)**, ranging from 10% to 20% of the font size. This mimics the inscription style of ancient monuments.

**Be Vietnam Pro** is used for functional UI text, buttons, and secondary information. Its contemporary construction ensures readability on small screens while its slightly warm curves complement the organic "swirl" motifs used in the iconography.

## Layout & Spacing

The layout follows a strict **vertical 9:16 mobile-first grid**. The central focus is a square 3x3 game board, which should be horizontally centered with a safe margin of 24px on either side.

- **The Game Board:** A fixed-aspect ratio container that occupies the center-third of the vertical screen.
- **Header/Footer:** Symmetrical placement of player profiles at the top and action menus at the bottom.
- **Rhythm:** An 8px base unit governs all padding and margins, ensuring a mathematical harmony that reflects the "strategic" nature of the game.

## Elevation & Depth

In this design system, depth is conveyed through **Light Emission** rather than physical shadows. Since the background is a dark emerald, depth is achieved by "stacking" gold elements with varying levels of glow and opacity.

1.  **Level 0 (Base):** The Deep Emerald background.
2.  **Level 1 (Sub-surface):** Muted Gold grid lines at 30% opacity, appearing etched into the emerald surface.
3.  **Level 2 (Active):** Game tokens (Pyramid/Swirl) rendered in Primary Gold with a subtle outer glow (4px blur, 15% opacity Primary Gold) to make them appear as if they are resting on the surface.
4.  **Level 3 (Highlight):** The winning strike line, rendered in Bright Gold with a "bloom" effect to signify victory.

## Shapes

The shape language is **geometric and sharp**, reinforcing the "strategic" and "architectural" themes. 

- **Containers:** Cards and modal windows use a very subtle corner radius (Soft, 4px) to prevent the UI from feeling overly aggressive while maintaining a serious tone.
- **Game Tokens:** The tokens themselves are strictly geometric.
    - **The Stepped Pyramid:** A series of three stacked horizontal bars of decreasing width, forming a triangle-like silhouette.
    - **The Swirl:** A precision-engineered spiral based on the golden ratio, composed of clean line segments rather than hand-drawn curves.
- **Buttons:** Rectangular with sharp or minimally softened edges, emphasizing a "stone-cut" aesthetic.

## Components

### Game Board
The 3x3 grid is constructed of Muted Gold lines (#C9A227) at 0.5pt thickness. The intersections should be clean, with no rounded joins.

### Player Tokens
- **X-Replacement (Temple):** A geometric line-art stepped pyramid. The lines should have a consistent stroke weight (2px).
- **O-Replacement (Swirl):** A minimalist curved swirl. The stroke weight must match the Temple icon to ensure visual balance.

### Primary Buttons
Buttons are outlined in Primary Gold with a transparent center. Upon being pressed, the button fills with a 10% Gold tint. The text inside is always Label-Caps with wide tracking.

### Scorecards
Minimalist containers located at the top of the screen. The "Active Player" is indicated by a Primary Gold underline (2px height) beneath their name, accompanied by a subtle breathing glow effect.

### Winning Strike
When a player wins, a Bright Gold (#F2C94C) line slices through the winning icons. This line should be 3px thick and feature a "tapered" start and end to mimic a quick, strategic stroke.