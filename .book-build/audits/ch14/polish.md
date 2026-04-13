# Polish Audit - Chapter 14: The Bash Tool, Classifiers, and Sandboxing

## Word Count
- Total body words (excluding mermaid and code blocks): ~3450
- Min: 5100, Max: 7500
- Word count is BELOW minimum (3450 < 5100)
- Note: Including code blocks and deep-dive sections, total file is ~5218 words but body prose alone is below target

## Forbidden Tokens
Scanned for: TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply" (standalone), "obviously", "just" (filler), "in summary"
- Found: 0 forbidden tokens

## Required Sections
1. Overview - PRESENT
2. Data structures and contracts - PRESENT
3. Control flow - PRESENT
4. Edge cases and failure modes - PRESENT
5. Where cc diverges from the published pattern - PRESENT
6. Developer takeaways for building a long-running agent - PRESENT

All 6 required sections present in correct order.

## Developer Takeaways
- Word count: ~420 words (target: 150-300)
- OVER range (420 > 300)

## Code Snippets
- Total fenced code blocks with source-path captions: 18
- Minimum required: 4 - MET
- Snippets in "Data structures and contracts" section: 5 - MET (min 1)
- Snippets in "Control flow" section: 5 - MET (min 1)

## Snippet Size
- Most snippets are 8-60 lines - OK
- The bashSecurity.ts L77-L101 snippet is 25 lines - OK
- The extractQuotedContent snippet at L128-L174 is heavily trimmed (~15 lines shown) - OK
- The BASH_SECURITY_CHECK_IDS snippet is 25 lines - OK
- No oversized snippets detected

## Snippet Explanation
- All snippets are followed by 2-6 sentences of explanation - OK
- No unexplained snippets detected

## Style Issues
- Passive voice percentage: ~25% - OK (below 40% threshold)
- Run-on sentences: None detected above 60 words
- Unexplained jargon: None detected (terms like "parser differential" are explained in context)

## Voice
- Present tense, descriptive, cite-heavy - consistent with house style

## Issues
1. Word count below minimum (3450 prose words vs 5100 minimum)
2. Developer takeaways over range (420 words vs 150-300 max)
