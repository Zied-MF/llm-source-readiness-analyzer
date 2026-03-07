# Page Type: Category

## Goal
A category page should be evaluated based on how clearly it groups, compares, and presents multiple products, offers, services, or items within the same family.

This scoring must stay generalist and must not rely on industry-specific logic.

---

## Detection logic

Classify a page as `category` if the detection score is >= 3.

### Detection signals
- +2 if the page contains repeated item cards or repeated offer/product blocks
- +1 if the URL contains a category-like term:
  - `/category`
  - `/categories`
  - `/collection`
  - `/collections`
  - `/catalog`
  - `/shop`
  - `/services`
  - `/solutions`
  - `/plans`
  - `/pricing`
  - `/formations`
- +1 if the page contains multiple links to detail pages in the same family
- +1 if the page contains multiple repeated CTAs across item blocks
- +1 if the page contains comparison-like sections, filters, or grouped listings
- +1 if the H1 looks like a family/group name rather than a single product name

---

## Weights

- semanticHtml: 10
- headingStructure: 10
- extractability: 14
- answerFirst: 8
- chunkability: 14
- chunkAutonomy: 10
- factualSignals: 12
- faqReadiness: 8
- readability: 6
- consistency: 8

Total = 100

---

## Subscore behavior

### semanticHtml
Start at 50.

Rules:
- +20 if `main` or `article` exists
- +10 if at least 3 `section` elements exist
- +10 if the page has a clearly structured listing area
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

Useful category headings include:
- Compare plans
- Our services
- Pricing
- Features by plan
- Collections
- Delivery options
- Materials
- FAQ
- How to choose
- Included features

Vague headings include:
- Discover
- Explore
- Experience
- Learn more
- Inspiration
- Our vision
- Innovation

---

### extractability
Start at 50.

Use:
- mainContentLength
- totalTextLength
- mainRatio = mainContentLength / totalTextLength

Rules:
- +25 if mainRatio >= 0.25
- +15 if mainRatio >= 0.18
- -20 if mainRatio < 0.12
- +15 if mainContentLength >= 1200
- +10 if mainContentLength >= 700
- -20 if mainContentLength < 400
- -10 if there is too much navigation, filter, or footer noise
- -10 if the useful grouping/comparison information is hard to extract cleanly

---

### answerFirst
For category pages, this means category clarity.

Goal:
The top of the page should clearly explain what group of items the page contains and how to understand or choose among them.

Start at 40.

Analyze the first 200 to 300 useful words.

Rules:
- +25 if the hero or intro clearly states:
  - what this category/group is
  - what kinds of items are included
  - at least 1 useful qualifier or distinction
- +15 if the page explains how items differ or how to choose
- +10 if a strong navigational or comparison CTA is present
- -20 if the top of the page is only branding or vague category language
- -10 if the page starts without clearly framing the listing

---

### chunkability
Start at 50.

Rules:
- +20 if repeated item blocks are clearly structured
- +10 if comparison tables, grouped cards, or plan grids are present
- +10 if lists are present
- +10 if major themes are clearly separated
- +10 if each group/listing block is short and scannable
- -20 if the page is a wall of mixed listings and text
- -10 if the structure makes it hard to compare items

---

### chunkAutonomy
Start at 50.

Rules:
- +20 if headings are self-explanatory
- +15 if each listing or major section provides enough context on its own
- +10 if each block maps to a clear browse/select intent
- +10 if grouped items are clearly distinguishable
- -15 if too many blocks depend on visuals only
- -10 if repeated blocks are too vague or too generic without clear descriptors

Clear intents include:
- compare options
- browse a family of items
- choose the right offer
- understand differences
- view included features

---

### factualSignals
For category pages, this means category facts and decision signals.

Start at 30.

Rules:
- +20 if multiple concrete item attributes are present
- +15 if prices, plans, tiers, or ranges are present
- +15 if comparison or grouped feature information is present
- +10 if cards, lists, or tables group decision-making information well
- +10 if filters, variants, or subcategories are explicit
- -20 if the page is too vague and mostly promotional
- -10 if useful facts exist but are poorly structured

Do not require author or publication date.

---

### faqReadiness
FAQ is optional but recommended.

Start at 40.

Rules:
- +25 if a real FAQ exists
- +15 if headings are phrased as buyer or chooser questions
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
- +10 if lists or cards are present
- +5 if sentences are reasonably short
- -15 if many paragraphs exceed 180 words
- -10 if the page mixes too much dense text with dense listings

---

### consistency
For category pages, this means category consistency.

Start at 70.

Rules:
- +15 if title, meta description, H1, and sections all refer to the same family/group
- +10 if the page helps compare or browse items logically
- +10 if naming is consistent across blocks
- -15 if the page mixes unrelated item families
- -10 if the structure does not support selection or comparison
- -10 if the page is repetitive and unclear

---

## Forbidden issues for category pages

Do not generate these issues for `category` pages:
- No author information found
- No publication date found
- No answer-first intro found
- No citations or references found

Use more appropriate issues instead, such as:
- Category intro does not clearly explain what is included
- Too few concrete decision-making details were detected
- Item blocks are hard to compare
- Key category information is buried in UI-heavy sections
- The page mixes too many different item groups
- Important listing details are present but poorly structured

---

## UI labels for category pages

- answerFirst -> Category clarity
- factualSignals -> Category facts & decision signals
- faqReadiness -> FAQ / chooser questions
- consistency -> Category consistency