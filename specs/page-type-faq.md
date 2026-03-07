# Page Type: FAQ

## Goal
A FAQ page should be evaluated based on how clearly it presents explicit questions and direct answers in a way that is easy for LLMs to extract, reuse, and cite.

This scoring must stay generalist and must not rely on industry-specific logic.

---

## Detection logic

Classify a page as `faq` if the detection score is >= 3.

### Detection signals
- +2 if JSON-LD contains `FAQPage`
- +1 if the URL contains an FAQ-like term:
  - `/faq`
  - `/faqs`
  - `/help`
  - `/questions`
  - `/support`
  - `/aide`
  - `/questions-frequentes`
- +1 if the page contains multiple question-like headings
- +1 if the page contains repeated question-answer blocks
- +1 if the H1 looks like an FAQ or support question page
- +1 if the page structure is clearly organized around explicit user questions

---

## Weights

- semanticHtml: 10
- headingStructure: 10
- extractability: 12
- answerFirst: 8
- chunkability: 14
- chunkAutonomy: 14
- factualSignals: 10
- faqReadiness: 12
- readability: 5
- consistency: 5

Total = 100

---

## Subscore behavior

### semanticHtml
Start at 50.

Rules:
- +20 if `main` or `article` exists
- +10 if at least 3 `section` elements exist
- +10 if `FAQPage` schema exists
- +10 if extracted main content length > 800 characters
- -20 if neither `main` nor `article` exists
- -15 if extracted main content length < 300 characters
- -10 if the page is mostly UI-heavy with too little useful text

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

Useful FAQ headings include:
- How does X work?
- What is X?
- Can I do X?
- When will X happen?
- Why does X happen?
- Troubleshooting questions
- Pricing questions
- Delivery questions

Vague headings include:
- Discover
- Explore
- Learn more
- More info
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
- +15 if mainRatio >= 0.22
- -20 if mainRatio < 0.14
- +15 if mainContentLength >= 1000
- +10 if mainContentLength >= 600
- -20 if mainContentLength < 300
- -10 if there is too much navigation, CTA, or footer noise
- -10 if Q&A blocks are hard to isolate cleanly

---

### answerFirst
For FAQ pages, this means question-page clarity.

Goal:
The top of the page should clearly explain what kinds of questions are answered here.

Start at 40.

Analyze the first 150 to 250 useful words.

Rules:
- +20 if the intro clearly explains the scope of the FAQ
- +15 if the page quickly presents explicit user questions
- +10 if the H1 makes the FAQ purpose obvious
- -20 if the top of the page is vague or branding-heavy
- -10 if the page starts without clarifying what questions are covered

---

### chunkability
Start at 50.

Rules:
- +25 if repeated Q&A blocks are clearly structured
- +15 if each answer is reasonably short and self-contained
- +10 if questions are easy to scan
- +10 if sections group related questions well
- -20 if answers are too long and dense
- -10 if questions and answers are mixed in an unclear layout

---

### chunkAutonomy
Start at 50.

Rules:
- +25 if each answer can stand on its own
- +15 if each question provides enough context
- +10 if related questions are grouped logically
- +10 if answers avoid vague references
- -15 if answers depend too much on other parts of the page
- -10 if many answers are too short to be useful
- -10 if too many answers are vague or incomplete

---

### factualSignals
For FAQ pages, this means directness and support signals.

Start at 30.

Rules:
- +20 if answers contain concrete details, conditions, limits, or examples
- +15 if policy, pricing, timing, delivery, setup, or support details are explicit
- +10 if answers include useful clarifiers or edge cases
- +10 if structured schema like `FAQPage` exists
- -20 if answers are too vague
- -10 if answers are overly promotional instead of informative

Author is not required.
Publication date is not required.

---

### faqReadiness
For FAQ pages, FAQ structure is expected.

Start at 40.

Rules:
- +30 if a real FAQ exists with repeated Q&A blocks
- +20 if multiple headings are phrased as explicit questions
- +10 if short, direct answers follow the questions
- +10 if `FAQPage` schema exists
- -25 if the page claims to be FAQ-like but has weak or unclear Q&A structure

---

### readability
Start at 70.

Rules:
- +10 if answers are short to medium
- +10 if questions are easy to scan
- +5 if sentences are reasonably short
- -15 if many answers exceed 180 words
- -10 if answers are too dense or repetitive

---

### consistency
For FAQ pages, this means FAQ consistency.

Start at 70.

Rules:
- +15 if title, meta description, H1, and Q&A blocks all refer to the same scope
- +10 if questions are logically grouped
- +10 if answers stay aligned with the user question
- -15 if the page mixes unrelated topics without clear grouping
- -10 if answers drift away from the questions
- -10 if the page is repetitive or confusing

---

## Recommended issues for FAQ pages

Appropriate issues include:
- FAQ scope is not clearly explained at the top
- Q&A blocks are weak or inconsistently structured
- Some answers are too vague to be useful for LLM reuse
- Questions are not grouped clearly
- Answers are too long and dense
- Important practical details are missing from the answers

Avoid forcing author/date-related issues on FAQ pages.

---

## UI labels for FAQ pages

- answerFirst -> FAQ scope clarity
- factualSignals -> Directness & support signals
- faqReadiness -> FAQ structure
- consistency -> FAQ consistency