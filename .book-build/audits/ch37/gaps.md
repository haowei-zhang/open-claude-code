# Gaps Audit: Chapter 37

## Brief Verification

**Synopsis**: "Execution pipelines per hook type, registry, async hook registry, SSRF guard, session hooks."
The chapter covers: execution pipelines for all 5 hook types, session hook registry, SSRF guard, session hooks. The "async hook registry" is mentioned briefly in the command hook section (AsyncHookRegistry, registerPendingAsyncHook) but is not covered in depth. This is a minor gap.

## Source File Citation Check

| Source File | Cited? |
|---|---|
| src/utils/hooks/execPromptHook.ts | Yes (snippet + discussion) |
| src/utils/hooks/execAgentHook.ts | Yes (snippet + discussion) |
| src/utils/hooks/execHttpHook.ts | Yes (snippet + discussion) |
| src/utils/hooks/sessionHooks.ts | Yes (snippet + discussion) |
| src/utils/hooks/registerSkillHooks.ts | Yes (snippet + discussion) |
| src/utils/hooks/hookEvents.ts | Yes (snippet + discussion) |
| src/utils/hooks/ssrfGuard.ts | Yes (snippet + discussion) |

All 7 source files cited. No uncited brief files.

## Required Diagrams

| Required | Found? |
|---|---|
| (a) sequenceDiagram of a prompt hook | Yes |
| (b) sequenceDiagram of an HTTP hook with SSRF check | Yes |

Both required diagrams present.

## Minimum Counts

- Citation count: 9 (>= 6, pass)
- Diagram count: 2 (>= 2, pass)
- Snippet count: 9 (>= 4, pass)

## Top Source Files Without Snippets

Top 3 source files (most central to the chapter topic):
1. src/utils/hooks/execHttpHook.ts — has snippet (pass)
2. src/utils/hooks/execPromptHook.ts — has snippet (pass)
3. src/utils/hooks/execAgentHook.ts — has snippet (pass)

All top files have snippets.

## Uncovered Topics

1. **Async hook registry**: The chapter mentions `AsyncHookRegistry` and `registerPendingAsyncHook` in passing in the command hook section but does not provide a dedicated discussion of the async hook lifecycle, how async hooks re-wake the agent, or the registry's data structure. This is a gap given the synopsis mentions "async hook registry" explicitly.

2. **Command hook execution detail**: The chapter describes command hook execution conceptually (spawn subprocess, pipe stdin, read exit code) but does not include a code snippet from the main `hooks.ts` file showing the actual subprocess spawning logic. This is because the main hooks.ts file is very large and the exec modules are in separate files, but a reader would expect to see the central dispatch code.

## Verdict: revise (1 uncovered topic from the brief's synopsis — async hook registry)
