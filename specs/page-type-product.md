# Page Type: Product

## Goal
A product page should be evaluated based on how clearly it presents a product or offer, its concrete attributes, its offer-related information, and its reusable content blocks for LLMs.

This scoring must stay generalist and must not rely on industry-specific logic.

---

## Detection logic

Classify a page as `product` if the detection score is >= 3.

### Detection signals
- +2 if JSON-LD contains `Product` or `Offer`
- +1 if the URL contains a product-like term:
  - `/product`
  - `/products`
  - `/produit`
  - `/offre`
  - `/pricing`
  - `/tarifs`
  - `/solution`
- +1 if the H1 looks like a product or offer name
- +1 if the page contains at least 2 transactional CTAs
- +1 if the page contains at least 3 measurable or concrete attributes
- +1 if the page contains sections typical of product evaluation:
  - features
  - specifications
  - pricing
  - options
  - plans
  - integrations
  - dimensions
  - materials
  - ingredients
  - delivery
  - setup
  - compatibility

### Transactional CTA examples
- buy
- order
- add to cart
- book
- request a quote
- request a demo
- start
- try
- subscribe
- see pricing
- configure

---

## Weights

- semanticHtml: 10
- headingStructure: 10
- extractability: 15
- answerFirst: 10
- chunkability: 12
- chunkAutonomy: 10
- factualSignals: 15
- faqReadiness: 6
- readability: 5
- consistency: 7

Total = 100

---

## Subscore behavior

### semanticHtml
Start at 50.

Rules:
- +20 if `main` or `article` exists
- +10 if at least 3 `section` elements exist
- +10 if `Product` or `Offer` schema exists
- +10 if extracted main content length > 1200 characters
- -20 if neither `main` nor `article` exists
- -15 if extracted main content length < 500 characters
- -10 if the page appears mostly visual or UI-heavy with too little useful text

Clamp between 0 and 100.

---

### headingStructure
Start at 100.

Rules:
- +5 if exactly 1 `h1`
- -20 if 0 `h1`
- -20 if more than 1 `h1`
- +10 if at least 2 `h2`
- -15 if no `h2`
- -10 for each heading hierarchy jump (example: `h2` -> `h4`)
- -10 if more than 50% of headings are vague

Useful product headings include:
- Features
- Pricing
- Specifications
- Dimensions
- Ingredients
- Integrations
- Security
- Setup
- Delivery
- Materials
- Options
- Plans
- Use cases

Vague headings include:
- Discover
- Explore
- Experience
- Learn more
- Our vision
- Why us
- The future
- Innovation

---

### extractability
Start at 50.

Use:
- mainContentLength
- totalTextLength
- mainRatio = mainContentLength / totalTextLength

Rules:
- +25 if mainRatio >= 0.30
- +15 if mainRatio >= 0.20
- -20 if mainRatio < 0.15
- +15 if mainContentLength >= 1500
- +10 if mainContentLength >= 800
- -20 if mainContentLength < 500
- -10 if there is too much navigation, CTA, or footer noise
- -10 if key information appears hidden inside hard-to-extract UI blocks

---

### answerFirst
For product pages, this means hero clarity.

Goal:
The top of the page should clearly explain what the product or offer is and what it provides.

Start at 40.

Analyze the first 250 to 350 useful words.

Rules:
- +25 if the hero clearly states:
  - the product or offer name
  - the nature of the product or service
  - at least 1 concrete attribute
- +15 if the hero summarizes the offer clearly
- +10 if a clear transactional CTA is present near the hero
- -20 if the hero is only a vague slogan
- -10 if the top of the page is mostly branding, emotion, or visuals with too little useful information

Good examples:
- CRM for small businesses with email automation and visual pipelines
- Ergonomic office chair with adjustable lumbar support
- 8-week SEO training with coaching and templates

Bad examples:
- Reinvent your everyday life
- Enter a new experience
- Innovation for your ambitions

---

### chunkability
Start at 50.

Rules:
- +20 if most sections are between 60 and 250 words
- +10 if paragraphs are short to medium
- +10 if lists are present
- +10 if tables, spec sheets, plans, or comparisons are present
- +10 if major themes are clearly separated
- -20 if there are large walls of text
- -10 if sections are too long and poorly broken up

---

### chunkAutonomy
Start at 50.

Rules:
- +20 if headings are self-explanatory
- +15 if the first paragraph of each major section provides context
- +10 if specialized terms are explained
- +10 if sections map clearly to a user or buyer intent
- -15 if too many sections depend on visuals to be understood
- -10 if there are many vague references without context

Examples of clear intents:
- what it is
- how it works
- what is included
- how much it costs
- which options exist
- who it is for
- how to get it or activate it

---

### factualSignals
For product pages, this means product facts and trust signals.

Do not require author or publication date.

Start at 30.

Rules:
- +20 if at least 3 concrete or measurable facts are present
- +15 if pricing, plans, offers, or ranges are present
- +15 if structured product characteristics are present
- +10 if lists, cards, or tables group information well
- +15 if `Product` or `Offer` schema exists
- +10 if variants, plans, formats, or options are explicitly presented
- -20 if the page is mostly marketing with too few concrete facts
- -10 if important facts are present but poorly structured

Examples of concrete facts:
- price
- duration
- dimensions
- weight
- ingredients
- compatibility
- capacity
- shipping time
- number of features
- format
- limits
- included options
- warranty
- delivery

---

### faqReadiness
FAQ is optional but recommended.

Start at 40.

Rules:
- +25 if a real FAQ exists
- +15 if headings are phrased as questions
- +10 if short answers follow those questions
- +10 if FAQPage schema exists
- 0 if no FAQ exists

Do not apply a strong penalty if FAQ is absent.

A soft recommendation is acceptable.

---

### readability
Start at 70.

Rules:
- +10 if paragraphs are short to medium
- +10 if lists are present
- +5 if sentences are reasonably short
- -15 if many paragraphs exceed 180 words
- -10 if many sentences are repeatedly too long

Be less strict than for article pages.

---

### consistency
For product pages, this means product consistency.

Start at 70.

Rules:
- +15 if title, meta description, H1, and sections all refer to the same product or offer
- +10 if sections cover logical evaluation intents
- +10 if the product or offer name is consistent throughout the page
- -15 if the intro, headings, and later content pull in different directions
- -10 if the page is overly repetitive and marketing-heavy
- -10 if multiple offers are mixed without a clear hierarchy

---

## Forbidden issues for product pages

Do not generate these issues for `product` pages:
- No author information found
- No publication date found
- No answer-first intro found
- No citations or references found

Use more appropriate issues instead, such as:
- Hero section does not clearly explain the product
- Too few concrete product details were detected
- Key information is buried in UI-heavy sections
- Section headings do not map clearly to buyer intents
- Important product facts are present but poorly structured
- Main content is difficult to extract cleanly

---

## UI labels for product pages

- answerFirst -> Hero clarity
- factualSignals -> Product facts & trust signals
- faqReadiness -> FAQ / buyer questions
- consistency -> Product consistency