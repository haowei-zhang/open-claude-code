# Consistency Audit: Chapter 10 — Token Budgets, Effort, and Fast Mode

## Terminology Check

| Registry Term | Used in Chapter | Canonical? | Notes |
|---------------|----------------|------------|-------|
| tool | Yes | Yes | Used correctly |
| subagent | Yes | Yes | Used correctly (subagent budget isolation) |
| compaction | Yes | Yes | Used correctly (compaction pipeline) |
| observation masking | Yes | Yes | Used correctly (52% cost reduction) |
| microcompact | Yes | Yes | Referenced (IMAGE_MAX_TOKEN_SIZE alignment) |
| query loop | Yes | Yes | Used correctly |
| back-pressure | No | N/A | Not relevant to this chapter |
| fast-path routing | No | N/A | Not relevant to this chapter |

## Term Conflict Analysis

- "token budget" — used consistently throughout; not in registry but used as a compound term referencing the system under discussion. No conflict.
- "effort" / "effortValue" — used correctly, matching the EffortValue type import. No conflict.
- "fast mode" / "fastMode" — used consistently. The chapter uses "fast mode" as the user-facing term and `fastMode` as the code field. Consistent.
- "rough estimation" vs "roughTokenCountEstimation" — function name used correctly. No conflict.
- "diminishing returns" — used consistently. Not a registry term.

No term conflicts detected. The chapter uses all registered terms according to their canonical definitions.

## Voice Drift Analysis

The chapter is written in present tense, descriptive, cite-heavy style — matching the house style. No voice drift detected.

## Proposed New Terms

1. **token budget** — A client-side limit on the number of tokens consumed per agentic turn, enforced by BudgetTracker and checkTokenBudget() to detect diminishing returns and budget exhaustion.
2. **diminishing-returns detector** — The heuristic in checkTokenBudget() that compares token deltas across consecutive loop iterations, stopping the loop when progress falls below the DIMINISHING_THRESHOLD for at least 3 continuations.
3. **token estimation pipeline** — The three-tier counting system (API-based, Haiku fallback, rough heuristic) that provides token counts with decreasing accuracy and latency, used when the primary API endpoint is unavailable.
