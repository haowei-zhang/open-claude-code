# Gaps Audit: Chapter 22 — Tasks: A Durable Unit of Work

## Brief Synopsis Match

The chapter's Overview matches the brief synopsis: "Task records, status transitions, blocking relationships, filesystem locking, storage paths, high-water mark, the seven task types." All topics are covered.

## Uncited Brief Files

The following files from the brief are not cited in the chapter:
- `src/tools/TaskCreateTool/constants.ts` — Contains only the tool name constant
- `src/tools/TaskCreateTool/prompt.ts` — Contains the tool description/prompt text
- `src/tools/TaskListTool/constants.ts` — Contains only the tool name constant
- `src/tools/TaskListTool/prompt.ts` — Contains the tool description/prompt text
- `src/tools/TaskGetTool/constants.ts` — Contains only the tool name constant
- `src/tools/TaskGetTool/prompt.ts` — Contains the tool description/prompt text
- `src/tools/TaskUpdateTool/constants.ts` — Contains only the tool name constant
- `src/tools/TaskUpdateTool/prompt.ts` — Contains the tool description/prompt text
- `src/tools/TaskStopTool/prompt.ts` — Contains the tool description/prompt text
- `src/tools/TaskStopTool/UI.tsx` — Contains the tool's terminal UI rendering
- `src/tools/TaskOutputTool/constants.ts` — Contains only the tool name constant

These are all supporting files (constants, prompts, UI rendering) that follow the standard directory contract pattern. Their omission is expected for a chapter focused on architecture and control flow rather than individual tool prompts. However, 11 uncited files is notable.

## Missing Diagrams

None. All 3 mandated diagram types are present (sequenceDiagram, stateDiagram-v2, classDiagram).

## Minimum Counts

- Citation count: 70 (minimum 6) — PASS
- Diagram count: 3 (minimum 2) — PASS
- Snippet count: 15 (minimum 4) — PASS

## Top Files Without Snippets

The top 3 source files (src/utils/tasks.ts, src/tools/TaskCreateTool/TaskCreateTool.ts, src/tools/TaskUpdateTool/TaskUpdateTool.ts) all have snippets. PASS.

## Uncovered Topics

1. **TaskStopTool's KillShell backward compatibility**: The chapter mentions TaskStopTool but does not discuss the deprecated `shell_id` parameter or the `KillShell` alias, which is an important backward-compatibility story.
2. **TaskOutputTool's blocking wait**: The chapter mentions TaskOutputTool but does not explain its blocking/timeout semantics, which are key to how agents wait for subagent results.
3. **AppState task registry vs filesystem**: The chapter describes both the filesystem persistence and the in-memory AppState registry but does not clearly explain how they synchronize or why both exist.

## Verdict: revise

11 uncited brief files (though all are supporting files), 3 uncovered topics. The chapter covers its core topic well but has gaps in coverage of secondary tools and the dual-state architecture.
