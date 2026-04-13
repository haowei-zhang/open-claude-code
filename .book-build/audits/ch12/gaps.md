# Gaps Audit Report for Chapter 12: Tool Dispatch Pipeline

## Synopsis Match
The chapter's Overview matches the brief synopsis: it traces the full dispatch flow from `runTools()` through `runToolUse()`, the hook lifecycle, the streaming executor, and the permission decision hook. The synopsis mentions "validateInput → canUseTool → runPreToolUseHooks → tool.call → runPostToolUseHooks → result normalization" and the chapter covers all these stages.

## Source File Citation Check
Brief source files:
1. `src/services/tools/toolOrchestration.ts` — **CITED** (multiple snippets)
2. `src/services/tools/toolExecution.ts` — **CITED** (multiple snippets and references)
3. `src/services/tools/toolHooks.ts` — **CITED** (multiple snippets)
4. `src/services/tools/StreamingToolExecutor.ts` — **CITED** (multiple snippets)
5. `src/hooks/useCanUseTool.tsx` — **CITED** (snippets and references)

All 5 source files cited at least once. No uncited brief files.

## Mandated Diagrams Check
Brief requires:
- (a) sequenceDiagram of a successful tool dispatch — **NOT FOUND** (chapter has flowchart instead)
- (b) sequenceDiagram of a denied dispatch — **NOT FOUND**
- (c) stateDiagram-v2 of deferral / concurrency scheduling — **NOT FOUND**

The chapter has 2 flowcharts but they do not match the required diagram types.

## Minimum Counts
- Citation count: 21 (minimum: 6) — **PASS**
- Diagram count: 2 (minimum: 2) — **PASS** (count is met, but types don't match brief)
- Snippet count: 21 (minimum: 4) — **PASS**

## Top 3 Source Files Without Snippets
The top 3 source files (most central) are:
1. `src/services/tools/toolExecution.ts` — has snippets (the main pipeline code)
2. `src/services/tools/toolOrchestration.ts` — has snippets (partition, concurrent, serial)
3. `src/services/tools/StreamingToolExecutor.ts` — has snippets (canExecuteTool, TrackedTool, etc.)

No top files without snippets.

## Uncovered Topics
1. **Result normalization** — The brief mentions "result normalization" as the final step of the pipeline. The chapter discusses `processToolResultBlock` and `mapToolResultToToolResultBlockParam` in the tool execution code but does not explicitly describe the normalization step. This is a gap.
2. **Denied dispatch flow** — The brief requires a sequenceDiagram for a denied dispatch, but the chapter does not have a dedicated section or diagram for the denied flow. The permission-denied path is described textually but not diagrammed.
3. **Concurrency scheduling state machine** — The brief requires a stateDiagram-v2 for deferral/concurrency scheduling, but the chapter uses a flowchart instead. The state transitions (queued → executing → completed → yielded) are described but not shown as a state diagram.
