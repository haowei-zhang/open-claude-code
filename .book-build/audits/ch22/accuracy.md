# Accuracy Audit: Chapter 22 — Tasks: A Durable Unit of Work

## Summary

The chapter accurately describes the task system. All 28 citations point to valid files and line ranges. No hallucinated snippets were found. However, 12 of 15 snippets exhibit minor drift (omitted inline comments, whitespace differences, simplified code). The core identifiers, logic flow, and structural accuracy are preserved across all snippets.

## Snippet Verification

| # | Cited Range | Verdict | Notes |
|---|------------|---------|-------|
| 1 | tasks.ts:L76-L89 | drift | Inline comments stripped from TaskSchema |
| 2 | tasks.ts:L69-L74 | drift | Blank line added between TASK_STATUSES and TaskStatusSchema |
| 3 | tasks.ts:L199-L210 | drift | Multi-line comment omitted |
| 4 | tasks.ts:L221-L231 | verbatim | Content matches source |
| 5 | tasks.ts:L488-L499 | drift | Extra blank line before busyWithTasks |
| 6 | tasks.ts:L18 | verbatim | Single-line signal matches |
| 7 | tasks.ts:L61-L67 | verbatim | notifyTasksUpdated matches |
| 8 | tasks.ts:L284-L308 | drift | Comments and blank lines omitted |
| 9 | tasks.ts:L98-L108 | verbatim | LOCK_OPTIONS matches |
| 10 | tasks.ts:L110-L131 | drift | Range extends slightly past function |
| 11 | tasks.ts:L511-L523 | drift | 2-line comment omitted |
| 12 | TaskCreateTool.ts:L80-L114 | drift | setAppState call omitted |
| 13 | TaskUpdateTool.ts:L33-L66 | drift | .describe() text simplified |
| 14 | TaskUpdateTool.ts:L188-L199 | verbatim | Auto-assign owner matches |
| 15 | TaskListTool.ts:L72-L83 | drift | Content matches but formatting simplified |

## Uncited Sources

The chapter does not cite constants.ts, prompt.ts, or UI.tsx files from any tool directory. These are supporting files (prompts and UI rendering) that are not central to the chapter's topic and their omission is acceptable.

## Verdict: revise

12 snippets have drift (omitted comments or minor formatting differences). No hallucinated snippets. All citations verified.
