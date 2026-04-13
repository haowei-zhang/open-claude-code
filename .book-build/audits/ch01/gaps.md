# Gaps Audit: Chapter 01

## Overview vs Synopsis
The chapter's Overview positions cc in the emerging discipline of harness engineering and states the book's thesis that cc is the most mature public example of a long-running agent harness. This matches the brief synopsis exactly.

## Source File Citation Check
| Source File | Cited? | Location |
|-------------|--------|----------|
| src/main.tsx | Yes | Multiple: L85, L88 (snippet), L168 |
| src/entrypoints/cli.tsx | Yes | Multiple: L27, L29 (snippet), L43, L48 (snippet), L168, L173 (snippet) |
| README.md | Yes | L166 |

No uncited brief files.

## Mandated Diagrams
| Required | Found | Notes |
|----------|-------|-------|
| (a) flowchart model + harness = agent block diagram | Yes | Diagram 1: Prompt->Context->Harness with Agent = Model + Harness |
| (b) timeline flowchart METR task-duration doubling | Yes | Diagram 3: 2024-2026 task duration escalation |

No missing diagrams.

## Minimum Counts
| Metric | Required | Actual | Met? |
|--------|----------|--------|------|
| Citations | 6 | 7 | Yes |
| Diagrams | 2 | 3 | Yes |
| Snippets | 4 | 4 | Yes |

## Top Files Without Snippets
- src/entrypoints/cli.tsx: Has 3 snippets. OK.
- src/main.tsx: Has 1 snippet. OK.
- README.md: No fenced code snippet, but content is directly quoted in text (line 166). README.md is a Markdown documentation file, not a source code file, so it would not naturally produce caption-commented code blocks. The text quote serves the same purpose.

No significant gaps from top files.

## Uncovered Topics
No topics expected from the brief that are missing. The chapter covers: harness engineering thesis, Agent = Model + Harness equation, evolution from prompt to context to harness engineering, TerminalBench evidence, METR task-duration data, compound failure math, failure modes, divergence from published patterns, and developer takeaways.
