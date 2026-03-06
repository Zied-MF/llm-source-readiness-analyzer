# Scoring Model

## Overall score formula

overall =
0.10 * semanticHtml +
0.10 * headingStructure +
0.15 * extractability +
0.10 * answerFirst +
0.15 * chunkability +
0.10 * chunkAutonomy +
0.10 * factualSignals +
0.05 * faqReadiness +
0.10 * readability +
0.05 * consistency

## Subscores
- semanticHtml
- headingStructure
- extractability
- answerFirst
- chunkability
- chunkAutonomy
- factualSignals
- faqReadiness
- readability
- consistency

## Main rules
1. Exactly one H1 is positive
2. Zero or multiple H1 is negative
3. H2 presence is positive
4. Heading hierarchy jumps are negative
5. Explicit headings are positive
6. Vague headings are negative
7. Presence of main or article is positive
8. Strong main content length is positive
9. Weak main content is negative
10. Boilerplate-heavy pages are negative
11. Answer-first intro is positive
12. Vague marketing intro is negative
13. Good section sizes are positive
14. Very long paragraphs are negative
15. Lists are positive
16. Tables are positive
17. Autonomous sections are positive
18. Defined concepts are positive
19. Author/date/source/statistics are positive
20. FAQ or question headings are positive