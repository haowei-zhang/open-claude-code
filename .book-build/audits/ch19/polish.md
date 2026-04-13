# Polish Audit: Chapter 19 — Sync vs Fork vs Remote: The Execution Modes

## Word Count

Body word count (excluding mermaid and code blocks): 3826
Target range: [4675, 6875]
Verdict: **BELOW MIN** (3826 < 4675)

## Forbidden Tokens

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
- "in summary": 0

Total forbidden tokens: 0

## Mandatory Sections

1. Overview: PRESENT
2. Data structures and contracts: PRESENT
3. Control flow: PRESENT
4. Edge cases and failure modes: PRESENT
5. Where cc diverges from the published pattern: PRESENT
6. Developer takeaways for building a long-running agent: PRESENT

All 6 mandatory sections present and in correct order.

## Developer Takeaways

Word count: 646
Required range: 150-300
Verdict: **OVER RANGE** (646 > 300)

## Code Snippets

Total snippets with source-path captions: 14
Minimum required: 4

Snippets in "Data structures and contracts" section: 6 (FORK_AGENT, CacheSafeParams, ForkedAgentParams, RemoteAgentTaskState, LocalAgentTaskState, ProgressTracker)
Snippets in "Control flow" section: 6 (buildForkedMessages, FORK_PLACEHOLDER_RESULT, buildChildMessage, isForkSubagentEnabled, buildWorktreeNotice, pushApiMetricsEntry)

Both sections have >= 1 snippet.

## Oversized Snippets

None. All snippets are within 8-60 lines except:
- Snippet 8 (FORK_PLACEHOLDER_RESULT): 2 lines — **UNDERSIZED**

## Unexplained Snippets

All snippets are followed by 2-6 sentences of explanation. No unexplained snippets detected.

## Style Issues

- Takeaways section is 646 words, far exceeding the 150-300 word limit. Should be condensed.
- Word count is 3826, below the 4675 minimum. The chapter is 849 words short.

## Summary

- Word count: 3826 (below min of 4675)
- Forbidden tokens: 0
- Snippet count: 14
- Undersized snippet: 1 (snippet 8 at 2 lines)
- Takeaways word count: 646 (over 300 max)
