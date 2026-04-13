# Gaps Audit: Chapter 6 — `main.tsx` and the Command Router

## Synopsis Match
- Chapter overview matches the brief synopsis: deep dive into main.tsx, CLI subcommands, parsing, renderAndRun, headless vs interactive bifurcation. All topics covered.

## Source File Citation
- **Brief source files**: `src/main.tsx`
- **Cited in chapter**: YES — extensively cited (28 line references)
- **Uncited brief files**: None

## Required Diagrams

| Required | Found |
|----------|-------|
| (a) flowchart of command routing decisions | YES |
| (b) sequenceDiagram headless vs interactive launch | YES |

All mandated diagrams present.

## Minimum Counts

| Metric | Required | Actual | Pass? |
|--------|----------|--------|-------|
| Citations | 6 | 28 | YES |
| Diagrams | 2 | 2 | YES |
| Snippets | 4 | 4 | YES |

## Top Files Without Snippets
- Only one source file listed (`src/main.tsx`) — it has 4 snippets. PASS.

## Uncovered Topics
- The chapter does not cover the `init()` function (`src/entrypoints/init.ts`) in detail, though it is referenced. However, init.ts is the subject of chapter 5, so this is appropriate scoping.
- The chapter briefly mentions `renderAndRun()` but does not deeply explore the Ink rendering pipeline (covered in chapter 42). This is appropriate cross-referencing, not a gap.
