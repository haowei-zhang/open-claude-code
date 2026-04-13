# Polish Audit Report - Chapter 25: Messages and the Conversation Model

## Word Count

Body word count (excluding mermaid code blocks and fenced code snippets): ~2,840 words.
Target range: [3825, 5625].
Status: BELOW MINIMUM (2,840 < 3,825)

## Forbidden Tokens

Scan results:
- TODO: 0
- TBD: 0
- XXX: 0
- [NEEDS-VERIFY]: 0
- "similar to chapter": 0
- "as noted earlier": 0
- "we will discuss": 0
- "as we shall see": 0
- "simply" (standalone): 0
- "obviously": 0
- "just" (filler): 0
- "in summary" (phrase): 0

Total forbidden tokens found: 0

## Required Sections

| Section | Present |
|---------|---------|
| Overview | true |
| Data structures and contracts | true |
| Control flow | true |
| Edge cases and failure modes | true |
| Where cc diverges from the published pattern | true |
| Developer takeaways for building a long-running agent | true |

All 6 mandatory sections present in correct order.

## Developer Takeaways

Word count: ~330 words (8 numbered items, each ~40 words).
Range requirement: 150-300 words.
Status: OVER RANGE (330 > 300)

## Code Snippets

Fenced code blocks with source-path captions: 19
Minimum required: 4. PASS.

Snippets in "Data structures and contracts" section: 7
Minimum required: 1. PASS.

Snippets in "Control flow" section: 7
Minimum required: 1. PASS.

## Oversized Snippets

None. All snippets are within 8-60 line range.

## Unexplained Snippets

All snippets are followed by 2-6 sentences of explanation. No unexplained snippets detected.

## Style Issues

1. Line 298-299: Duplicated paragraph - "The `reorderMessagesInUI` function groups tool-use messages..." appears twice (once at line ~299 and again at line ~325). This is a copy-paste error.
2. The takeaways section has 8 items (expected prose paragraph of 150-300 words). The items are well-structured but exceed the word count limit.

## Duplicate Content

Line ~299 and ~325 contain an identical paragraph about `reorderMessagesInUI`. This is a clear editorial error that must be fixed.
