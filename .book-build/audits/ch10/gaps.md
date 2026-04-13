# Gaps Audit: Chapter 10 — Token Budgets, Effort, and Fast Mode

## Brief Synopsis Match

The chapter's Overview matches the brief synopsis: "How cc tracks and bounds token consumption, how 'effort' selects model/thinking, how fast mode flips the loop." All three topics are covered.

## Source File Citation Check

| Source File | Cited? |
|-------------|--------|
| src/query/tokenBudget.ts | Yes — extensively |
| src/services/tokenEstimation.ts | Yes — extensively |
| src/state/AppStateStore.ts | Yes — fastMode/effortValue fields |

All source files cited. No uncited brief files.

## Mandated Diagrams

| Required | Found? |
|----------|--------|
| (a) stateDiagram-v2 for effort levels | No — chapter has no stateDiagram-v2 |
| (b) flowchart of token-budget-triggered actions | Yes — checkTokenBudget flowchart |

One mandated diagram is missing: the effort-levels stateDiagram-v2.

## Minimum Counts

| Metric | Required | Actual | Pass? |
|--------|----------|--------|-------|
| Citations | 6 | 15 | Yes |
| Diagrams | 2 | 2 | Yes (count met, but type mismatch) |
| Snippets | 4 | 7 | Yes |

## Top Files Without Snippets

| Top Source File | Has Snippet? |
|-----------------|--------------|
| src/query/tokenBudget.ts | Yes (4 snippets) |
| src/services/tokenEstimation.ts | Yes (2 snippets) |
| src/state/AppStateStore.ts | Yes (1 snippet) |

All top source files have at least one snippet.

## Uncovered Topics

1. **Effort levels stateDiagram**: The brief requires a stateDiagram-v2 for effort levels, which would show the transitions between effort values and how they affect model selection. The chapter discusses effortValue in prose but does not include this diagram.

2. **Effort-to-model mapping**: The brief says effort "selects model/thinking." The chapter mentions that effortValue "provides a finer-grained control over the agent's diligence level" and "maps to different levels of thoroughness" but does not detail the actual mapping from EffortValue to model/thinking configuration. This is a gap relative to the brief.

3. **How fast mode "flips the loop"**: The brief says fast mode "flips the loop," implying a more significant behavioral change than just a system-prompt directive. The chapter describes fast mode as injecting a system-prompt directive, but does not discuss any loop-behavior changes beyond the prompt. The word "flips" suggests a structural change that the chapter does not substantiate.
