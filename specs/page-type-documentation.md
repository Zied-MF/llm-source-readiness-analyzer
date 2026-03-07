# Page Type: Documentation

## Goal
A documentation page should be evaluated based on how clearly it explains a system, feature, API, workflow, or procedure in a way that is precise, structured, and reusable by LLMs.

This scoring must stay generalist and must not rely on industry-specific logic.

---

## Detection logic

Classify a page as `documentation` if the detection score is >= 3.

### Detection signals
- +2 if JSON-LD contains `HowTo` or technical documentation-like structured data
- +1 if the URL contains a documentation-like term:
  - `/docs`
  - `/documentation`
  - `/doc`
  - `/kb`
  - `/knowledge-base`
  - `/manual`
  - `/api`
  - `/reference`
  - `/guide`
  - `/tutorial`
- +1 if the page contains code blocks, commands, parameter tables, or step-by-step sections
- +1 if the page contains multiple technical headings such as:
  - prerequisites
  - setup
  - installation
  - configuration
  - parameters
  - examples
  - reference
  - troubleshooting
- +1 if the H1 describes a feature, setup, API, or procedure
- +1 if the page is strongly text-structured with clear instructional sections

---

## Weights

- semanticHtml: 10
- headingStructure: 10
- extractability: 12
- answerFirst: 12
- chunkability: 12
- chunkAutonomy: 14
- factualSignals: 12
- faqReadiness: 6
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
- +10 if extracted main content length > 1000 characters
- +10 if code blocks, tables, or structured step blocks are present
- -20 if neither `main` nor `article` exists
- -15 if extracted main content length < 500 characters
- -10 if the page is mostly UI-heavy with too little useful instructional text

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

Useful documentation headings include:
- Prerequisites
- Installation
- Setup
- Configuration
- Parameters
- Response format
- Example
- Troubleshooting
- Notes
- Limitations
- Reference

Vague headings include:
- Discover
- Explore
- Learn more
- Insights
- Innovation
- The future

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
- +15 if mainContentLength >= 1400
- +10 if mainContentLength >= 800
- -20 if mainContentLength < 500
- -10 if there is too much navigation, sidebar, or footer noise
- -10 if useful instructional content is hard to isolate cleanly

---

### answerFirst
For documentation pages, this means procedural clarity.

Goal:
The beginning of the page should clearly state what the page helps the user do, understand, or configure.

Start at 40.

Analyze the first 200 to 300 useful words.

Rules:
- +25 if the intro clearly states the purpose of the page
- +15 if the intro explains what the user will learn, set up, or use
- +10 if prerequisites or scope are clearly stated near the top
- -20 if the intro is vague or generic
- -10 if the page starts without clearly framing the procedure or topic

Good examples:
- This guide explains how to configure webhook retries in X
- Use this endpoint to retrieve customer subscription data
- This page shows how to install and configure the SDK

Bad examples:
- Welcome to the future of development
- Discover a better way to work
- General intro with no procedural value

---

### chunkability
Start at 50.

Rules:
- +20 if steps, sections, or reference blocks are clearly separated
- +10 if lists are present
- +10 if code blocks, commands, or examples are present
- +10 if tables or parameter blocks are present
- +10 if sections are short and scannable
- -20 if the page is a wall of dense text
- -10 if multiple instructions are merged into unclear blocks

---

### chunkAutonomy
Start at 50.

Rules:
- +20 if headings are self-explanatory
- +15 if each section provides enough context on its own
- +15 if parameters, examples, or steps can be reused independently
- +10 if technical terms are defined or contextualized
- -15 if sections depend too much on previous context to be understood
- -10 if jargon is dense and unexplained
- -10 if examples are missing where needed

---

### factualSignals
For documentation pages, this means precision and reference signals.

Start at 30.

Rules:
- +20 if parameters, commands, formats, limits, or technical details are present
- +15 if examples are present
- +15 if tables, reference blocks, or structured details are present
- +10 if version, update, or compatibility signals are present
- +10 if concrete constraints or expected outputs are present
- -20 if the page stays too vague
- -10 if instructions are assertive but underspecified

Author is not required.
Publication date is not required.
Version/update info is useful when available.

---

### faqReadiness
FAQ is optional.

Start at 40.

Rules:
- +20 if a real FAQ exists
- +10 if headings are phrased as troubleshooting or common questions
- +10 if short answers follow those questions
- +10 if FAQPage schema exists
- 0 if no FAQ exists

Do not strongly penalize documentation pages for missing FAQ.

Troubleshooting sections can serve a similar purpose.

---

### readability
Start at 70.

Rules:
- +10 if paragraphs are short to medium
- +10 if lists, steps, code blocks, or tables are present
- +5 if sentences are reasonably short
- -15 if many paragraphs exceed 180 words
- -10 if technical density is too high without structure

---

### consistency
For documentation pages, this means procedural consistency.

Start at 70.

Rules:
- +15 if title, meta description, H1, and sections all refer to the same procedure/topic
- +10 if sections follow a logical setup/use/reference flow
- +10 if naming is consistent throughout the page
- -15 if the page mixes unrelated procedures or concepts
- -10 if steps, parameters, and examples are misaligned
- -10 if the page feels repetitive or structurally confusing

---

## Recommended issues for documentation pages

Appropriate issues include:
- Intro does not clearly explain the procedure or topic
- Too few concrete technical details were detected
- Key examples or reference blocks are missing
- Sections depend too much on previous context
- Instructional content is hard to extract cleanly
- The page mixes multiple procedures without clear structure

Avoid forcing author/date-related issues on documentation pages.

---

## UI labels for documentation pages

- answerFirst -> Procedural clarity
- factualSignals -> Precision & reference signals
- faqReadiness -> FAQ / troubleshooting
- consistency -> Procedural consistency