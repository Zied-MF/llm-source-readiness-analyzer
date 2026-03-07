# Page Type: Marketing

## Goal
A marketing page should be evaluated based on how clearly it communicates a value proposition, explains an offer, structures persuasive information, and exposes reusable content blocks for LLMs.

This scoring must stay generalist and must not rely on industry-specific logic.

---

## Detection logic

Classify a page as `marketing` if the detection score is >= 3.

### Detection signals
- +1 if the URL contains a marketing-like term:
  - `/solution`
  - `/solutions`
  - `/service`
  - `/services`
  - `/platform`
  - `/about`
  - `/why`
  - `/business`
  - `/enterprise`
  - `/for-`
- +1 if the page has a strong hero section with headline + CTA
- +1 if the page contains multiple persuasive or benefit-oriented sections
- +1 if the page contains several repeated CTA blocks
- +1 if the H1 describes a promise, solution, or audience rather than a single product or article topic
- +1 if the page contains sections like:
  - why choose us
  - benefits
  - how it works
  - use cases
  - testimonials
  - results
  - industries
  - features overview

---

## Weights

- semanticHtml: 10
- headingStructure: 10
- extractability: 12
- answerFirst: 14
- chunkability: 12
- chunkAutonomy: 10
- factualSignals: 10
- faqReadiness: 10
- readability: 6
- consistency: 6

Total = 100

---

## Subscore behavior

### semanticHtml
Start at 50.

Rules:
- +20 if `main` or `article` exists
- +10 if at least 3 `section` elements exist
- +10 if there is a clearly structured hero + body layout
- +10 if extracted main content length > 1000 characters
- -20 if neither `main` nor `article` exists
- -15 if extracted main content length < 400 characters
- -10 if the page is mostly visual/UI-heavy with too little useful text

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

Useful marketing headings include:
- How it works
- Benefits
- Use cases
- Why choose us
- Results
- Pricing
- Features overview
- Industries
- Testimonials
- FAQ

Vague headings include:
- Discover
- Explore
- Experience
- The future
- Innovation
- Our vision
- Learn more

---

### extractability
Start at 50.

Use:
- mainContentLength
- totalTextLength
- mainRatio = mainContentLength / totalTextLength

Rules:
- +25 if mainRatio >= 0.22
- +15 if mainRatio >= 0.16
- -20 if mainRatio < 0.10
- +15 if mainContentLength >= 1200
- +10 if mainContentLength >= 700
- -20 if mainContentLength < 400
- -10 if there is too much navigation, CTA, or footer noise
- -10 if the useful persuasive content is hard to extract cleanly

---

### answerFirst
For marketing pages, this means value proposition clarity.

Goal:
The top of the page should clearly explain what is being offered, for whom, and why it matters.

Start at 40.

Analyze the first 200 to 300 useful words.

Rules:
- +25 if the hero clearly states:
  - what the offer/solution is
  - who it is for
  - at least 1 concrete benefit or differentiator
- +15 if the hero summarizes the offer clearly
- +10 if a clear CTA is present near the hero
- -20 if the hero is only a vague slogan
- -10 if the top of the page is mostly branding/emotion with too little useful information

Good examples:
- Analytics platform for ecommerce teams to track AI visibility
- Payroll software for small businesses with automated tax filing
- Managed IT services for multi-site retail companies

Bad examples:
- Reinvent your future
- The next generation of excellence
- Innovation that moves you forward

---

### chunkability
Start at 50.

Rules:
- +20 if major benefit/use-case sections are clearly separated
- +10 if lists, cards, or short blocks are present
- +10 if FAQ, testimonial, or proof blocks are present
- +10 if comparison, pricing, or feature overview blocks are present
- +10 if most sections are scannable
- -20 if the page is mostly long persuasive text with little structure
- -10 if many sections repeat similar content without adding clarity

---

### chunkAutonomy
Start at 50.

Rules:
- +20 if headings are self-explanatory
- +15 if the first paragraph of each section gives enough context
- +10 if sections map clearly to an informational or conversion intent
- +10 if proof/testimonial/use-case blocks can stand on their own
- -15 if too many sections depend on visuals only
- -10 if many sections use vague promise-heavy language without enough context

Clear intents include:
- understand the offer
- see who it is for
- understand benefits
- learn how it works
- compare value
- resolve objections

---

### factualSignals
For marketing pages, this means proof and trust signals.

Start at 30.

Rules:
- +20 if concrete results, metrics, numbers, or proof points are present
- +15 if testimonials, case studies, logos, or social proof are present
- +15 if pricing, plans, or concrete feature details are present
- +10 if comparisons, process steps, or examples are present
- -20 if the page is too vague and mostly promotional
- -10 if claims are strong but weakly supported

Do not require author or publication date.

---

### faqReadiness
FAQ is optional but recommended.

Start at 40.

Rules:
- +25 if a real FAQ exists
- +15 if headings are phrased as buyer or objection-handling questions
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
- +10 if lists, cards, or short blocks are present
- +5 if sentences are reasonably short
- -15 if many paragraphs exceed 180 words
- -10 if persuasive text is too dense or repetitive

---

### consistency
For marketing pages, this means offer consistency.

Start at 70.

Rules:
- +15 if title, meta description, H1, and sections all refer to the same offer and audience
- +10 if the page moves logically from value proposition to explanation/proof
- +10 if naming is consistent throughout the page
- -15 if the page mixes unrelated audiences or offers
- -10 if claims and sections pull in different directions
- -10 if repetition is excessive

---

## Forbidden issues for marketing pages

Do not generate these issues for `marketing` pages:
- No author information found
- No publication date found
- No answer-first intro found
- No citations or references found

Use more appropriate issues instead, such as:
- Hero section does not clearly explain the offer
- Too few concrete proof signals were detected
- The page is too promotional and not specific enough
- Section headings do not map clearly to user questions or objections
- Key value information is buried in UI-heavy sections
- Claims are present but weakly supported

---

## UI labels for marketing pages

- answerFirst -> Value proposition clarity
- factualSignals -> Proof & trust signals
- faqReadiness -> FAQ / objections
- consistency -> Offer consistency