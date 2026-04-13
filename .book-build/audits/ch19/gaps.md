# Gaps Audit: Chapter 19 — Sync vs Fork vs Remote: The Execution Modes

## Synopsis Match

Brief synopsis: "In-process sync agents, forked subagents, remote CCR agents. Tradeoffs, shared vs isolated state, communication paths."

Chapter Overview covers all three execution modes, their tradeoffs, shared vs isolated state, and communication paths. The Overview matches the synopsis.

## Uncited Brief Source Files

All 5 source files from the brief are cited:
- src/tools/AgentTool/runAgent.ts — CITED (snippet at L761-L768)
- src/tools/AgentTool/forkSubagent.ts — CITED (multiple snippets)
- src/utils/forkedAgent.ts — CITED (CacheSafeParams, ForkedAgentParams snippets)
- src/tasks/RemoteAgentTask/RemoteAgentTask.tsx — CITED (RemoteAgentTaskState, checkRemoteAgentEligibility snippets)
- src/tasks/LocalAgentTask/LocalAgentTask.tsx — CITED (LocalAgentTaskState, ProgressTracker snippets)

Uncited brief files: 0

## Mandated Diagrams

Brief requires:
- (a) sequenceDiagram for sync mode — FOUND
- (b) sequenceDiagram for fork mode — FOUND
- (c) sequenceDiagram for remote mode — FOUND

Missing diagrams: 0

## Minimum Counts

- Citation count: 14 (>= 6) — PASS
- Diagram count: 3 (>= 2) — PASS
- Snippet count: 14 (>= 4) — PASS

## Top 3 Source Files Without Snippets

Top 3 source files (most central to the chapter's topic):
1. src/tools/AgentTool/forkSubagent.ts — HAS snippets (7 snippets)
2. src/tools/AgentTool/runAgent.ts — HAS snippet (1 snippet)
3. src/utils/forkedAgent.ts — HAS snippets (2 snippets)

Top files without snippets: 0

## Uncovered Topics

1. **runAgent.ts sync path deep dive**: The chapter describes the sync execution flow in prose but only shows one small snippet from runAgent.ts (the pushApiMetricsEntry callback). The main runAgent() function body — including how it creates the subagent context, initializes MCP, and runs the nested query() — is described but not shown in code. The runAgent.ts file is 2436 total lines but only 1 snippet of 7 lines is shown. This is a gap for the chapter's most important source file.

2. **Remote agent polling loop implementation**: The chapter describes the polling loop in the sequence diagram and in prose, but no code from the actual polling implementation (pollRemoteSessionEvents, fetchSession) is shown. The reader cannot see how the poll cycle is implemented, how events are processed, or how timeouts are handled in code.

3. **Auto-background transition implementation**: The chapter describes the auto-background transition in prose (6 steps) but shows no code for `runAsyncAgentLifecycle` or the timer mechanism. This is an important feature that the chapter's own divergence section highlights as a key difference from HER.

## Summary

- Uncited brief files: 0
- Missing diagrams: 0
- Citation count: 14
- Diagram count: 3
- Snippet count: 14
- Top files without snippets: 0
- Uncovered topics: 3
