# Polish Audit: Chapter 10 — Token Budgets, Effort, and Fast Mode

## Word Count

Body text (excluding mermaid blocks and fenced code snippets): approximately 2,820 words.
Target range: [2975, 4375].
Word count is BELOW minimum by ~155 words.

## Forbidden Tokens

Searched for: TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply" (standalone), "obviously", "just" (filler), "in summary"

- "simply" — not found as standalone
- "obviously" — not found
- "just" — not found as filler
- "in summary" — not found
- No forbidden tokens detected. Count: 0.

## Mandatory Sections

| Section | Present |
|---------|---------|
| Overview | Yes |
| Data structures and contracts | Yes |
| Control flow | Yes |
| Edge cases and failure modes | Yes |
| Where cc diverges from the published pattern | Yes |
| Developer takeaways for building a long-running agent | Yes |

All 6 mandatory sections present in correct order.

## Developer Takeaways

7 numbered takeaways, approximately 340 words. This exceeds the 150-300 word range slightly.

## Code Snippets

- Total fenced code snippets with source-path captions: 7
- Snippets in "Data structures and contracts" section: 4 (BudgetTracker, ContinueDecision, StopDecision, AppState fastMode/effortValue)
- Snippets in "Control flow" section: 2 (COMPLETION_THRESHOLD, bytesPerTokenForFileType)
- Minimum 4: MET
- Minimum 1 in data structures: MET
- Minimum 1 in control flow: MET

### Snippet Lengths

| # | Lines | Oversized (>60)? |
|---|-------|-------------------|
| 1 | 6 | No |
| 2 | 8 | No |
| 3 | 10 | No |
| 4 | 5 | No |
| 5 | 2 | No (minimum is 8 — flagged as undersized) |
| 6 | 10 | No |
| 7 | 7 | No (minimum is 8 — flagged as undersized) |

Snippet #5 (thresholds, 2 lines) is undersized at 2 lines (minimum 8).
Snippet #7 (image/document estimate, 7 lines) is slightly undersized at 7 lines (minimum 8).

### Snippet Explanations

Each snippet is followed by 2-6 sentences of explanation. No unexplained snippets detected.

## Style Issues

- No run-on sentences (>60 words) detected
- Passive voice usage: within acceptable range
- No unexplained jargon
- Chapter is in present tense, descriptive, cite-heavy (matches house style)

## Issues

1. Word count below minimum (2820 vs 2975 minimum)
2. Developer takeaways slightly over 300 words (~340)
3. Snippet #5 is undersized (2 lines, minimum 8)
4. Snippet #7 is undersized (7 lines, minimum 8)
