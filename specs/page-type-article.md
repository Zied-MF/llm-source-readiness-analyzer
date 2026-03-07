# Page Type: Article

## Goal
An article page should be evaluated based on how clearly it explains a topic, answers a question, teaches, guides, or informs in a way that can be easily reused by LLMs.

This scoring must stay generalist and must not rely on industry-specific logic.

---

## Detection logic

Classify a page as `article` if the detection score is >= 3.

### Detection signals
- +2 if JSON-LD contains `Article`, `BlogPosting`, `NewsArticle`, or `HowTo`
- +1 if the URL contains an article-like term:
  - `/blog`
  - `/guide`
  - `/article`
  - `/news`
  - `/resources`
  - `/ressources`
  - `/learn`
  - `/how-to`
  - `/help`
  - `/advice`
- +1 if the H1 looks explanatory or question-based
- +1 if the page contains strong text-heavy main content
- +1 if the page contains multiple `h2` or `h3`
- +1 if author, publication date, or updated date is present
- +1 if headings use explanatory patterns such as:
  - what
  - why
  - how
  - benefits
  - steps
  - examples
  - mistakes
  - best practices
  - FAQ

---

## Weights

- semanticHtml: 10
- headingStructure: 10
- extractability: 12
- answerFirst: 14
- chunkability: 12
- chunkAutonomy: 12
- factualSignals: 12
- faqReadiness: 6
- readability: 7
- consistency: 5

Total = 100

---

## Subscore behavior

### semanticHtml
Start at 50.

Rules:
- +20 if `main` or `article` exists
- +10 if at least 3 `section` elements exist
- +10 if `Article`, `BlogPosting`, or `HowTo` schema exists
- +10 if extracted main content length > 1500 characters
- -20 if neither `main` nor `article` exists
- -15 if extracted main content length < 700 characters
- -10 if the page appears to have too little real textual content

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

Useful article headings include:
- What is X
- Why X matters
- How X works
- Benefits of X
- Common mistakes
- Examples
- Best practices
- Steps
- FAQ

Vague headings include:
- Discover
- Explore
- More info
- Learn more
- Insights
- Opportunities
- The future

---

### extractability
Start at 50.

Use:
- mainContentLength
- totalTextLength
- mainRatio = mainContentLength / totalTextLength

Rules:
- +25 if mainRatio >= 0.35
- +15 if mainRatio >= 0.25
- -20 if mainRatio < 0.15
- +15 if mainContentLength >= 1800
- +10 if mainContentLength >= 1000
- -20 if mainContentLength < 700
- -10 if there is too much navigation, CTA, or footer noise
- -10 if the main content appears diluted by boilerplate

---

### answerFirst
For article pages, this means answer-first clarity.

Goal:
The beginning of the page should quickly answer the topic, define the concept, or summarize the key point.

Start at 40.

Analyze the first 200 to 300 useful words.

Rules:
- +30 if the intro directly answers the topic
- +15 if the intro includes a clear definition
- +10 if a short summary or key takeaway block is present
- -20 if the intro is too generic, vague, or marketing-heavy
- -10 if the first paragraphs circle around the topic without clearly answering it

Good examples:
- X is...
- To do X, you need...
- The main difference between X and Y is...
- In short...

Bad examples:
- long generic opening before answering
- branding-heavy opening
- vague intro with no direct informational value

---

### chunkability
Start at 50.

Rules:
- +20 if most sections are between 80 and 300 words
- +10 if paragraphs are short to medium
- +10 if lists are present
- +10 if tables, comparisons, or step blocks are present
- +10 if ideas are clearly separated into sections
- -20 if there are large walls of text
- -10 if sections are too long or poorly broken up

---

### chunkAutonomy
Start at 50.

Rules:
- +20 if headings are self-explanatory
- +15 if the first paragraph of each section provides context
- +10 if key concepts are clearly defined
- +10 if each section answers an identifiable mini-question
- -15 if too many sections depend heavily on previous sections to be understood
- -10 if jargon is not explained
- -10 if there are many vague references without context

---

### factualSignals
For article pages, this means evidence and trust signals.

Start at 30.

Rules:
- +15 if author is present
- +15 if publication date or updated date is present
- +10 if statistics, numbers, or concrete data points are present
- +15 if sources, references, or proof signals are present
- +10 if `Article` or `HowTo` schema exists
- +10 if concrete examples are present
- -20 if the article stays too vague
- -10 if the tone is assertive but weakly supported

---

### faqReadiness
FAQ is optional.

Start at 40.

Rules:
- +20 if a real FAQ exists
- +10 if headings are phrased as questions
- +10 if short answers follow those questions
- +10 if FAQPage schema exists
- 0 if no FAQ exists

Do not strongly penalize article pages for missing FAQ.

---

### readability
Start at 70.

Rules:
- +10 if paragraphs are short to medium
- +10 if lists are present
- +5 if sentences are reasonably short
- -15 if many paragraphs exceed 180 words
- -10 if many sentences are repeatedly too long
- -10 if text density feels too heavy

---

### consistency
For article pages, this means topic consistency.

Start at 70.

Rules:
- +15 if title, meta description, H1, and intro all refer to the same topic
- +10 if sections follow a logical progression
- +10 if the page stays focused on a clear informational intent
- -15 if sections pull in multiple unrelated directions
- -10 if repetition is excessive
- -10 if multiple topics are mixed without clear hierarchy

---

## Recommended issues for article pages

Appropriate issues include:
- No answer-first intro found
- Intro does not clearly answer the main topic
- No author information found
- No publication or update date found
- Few evidence or trust signals detected
- Some sections are too long to be easily reused by LLMs
- The page mixes multiple topics without a clear structure

Avoid overly harsh FAQ-related issues for article pages.

---

## UI labels for article pages

- answerFirst -> Answer-first clarity
- factualSignals -> Evidence & trust signals
- faqReadiness -> FAQ / question coverage
- consistency -> Topic consistency