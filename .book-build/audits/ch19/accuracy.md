# Accuracy Audit: Chapter 19 — Sync vs Fork vs Remote: The Execution Modes

## Snippet Verification

### Snippet 1: forkSubagent.ts:L60-L71 (FORK_AGENT)
- File exists: YES
- Line range valid: YES
- Content: `whenToUse` truncated with `...` — trim marker, acceptable. Otherwise verbatim.
- Verdict: **verbatim** (with trim)

### Snippet 2: forkedAgent.ts:L57-L68 (CacheSafeParams)
- File exists: YES
- Line range valid: YES
- Content: Exact match with source.
- Verdict: **verbatim**

### Snippet 3: forkedAgent.ts:L83-L113 (ForkedAgentParams)
- File exists: YES
- Line range valid: YES
- Content: Exact match with source.
- Verdict: **verbatim**

### Snippet 4: RemoteAgentTask.tsx:L22-L59 (RemoteAgentTaskState)
- File exists: YES
- Line range valid: YES
- Content: Comments omitted/paraphrased from actual source (e.g., `/** Task-specific metadata (PR number, repo, etc.). */` omitted, `/** Long-running agent... */` omitted). Identifiers match. JSDoc comments differ.
- Verdict: **drift** (comments omitted but identifiers preserved)

### Snippet 5: LocalAgentTask.tsx:L116-L148 (LocalAgentTaskState)
- File exists: YES
- Line range valid: YES
- Content: `unregisterCleanup` field omitted from chapter. Several inline comments omitted. Field order differs slightly.
- Verdict: **drift** (field omitted, comments differ)

### Snippet 6: LocalAgentTask.tsx:L43-L57 (ProgressTracker)
- File exists: YES
- Line range: Off by 2 (actual starts at L41). Comments omitted from chapter version.
- Verdict: **drift** (line offset, comments omitted)

### Snippet 7: forkSubagent.ts:L107-L169 (buildForkedMessages)
- File exists: YES
- Line range valid: YES
- Content: Chapter omits the `if (toolUseBlocks.length === 0)` fallback block and the TODO comment. Main logic preserved.
- Verdict: **drift** (fallback path omitted)

### Snippet 8: forkSubagent.ts:L93 (FORK_PLACEHOLDER_RESULT)
- File exists: YES
- Content: Exact match.
- Verdict: **verbatim**

### Snippet 9: forkSubagent.ts:L171-L198 (buildChildMessage)
- File exists: YES
- Line range valid: YES
- Content: Chapter version is a paraphrased/simplified version of the actual boilerplate. Actual rules are longer and differ in wording.
- Verdict: **drift** (content paraphrased)

### Snippet 10: forkSubagent.ts:L32-L39 (isForkSubagentEnabled)
- File exists: YES
- Content: Exact match.
- Verdict: **verbatim**

### Snippet 11: forkSubagent.ts:L205-L210 (buildWorktreeNotice)
- File exists: YES
- Content: Exact match.
- Verdict: **verbatim**

### Snippet 12: RemoteAgentTask.tsx:L124-L141 (checkRemoteAgentEligibility)
- File exists: YES
- Content: Chapter shows `checkRemoteAgentEligibility()` with no parameters; actual function takes `{skipBundle = false}: {skipBundle?: boolean;} = {}`. Chapter shows `checkBackgroundRemoteSessionPrecondition({ skipBundle })` while actual is `checkBackgroundRemoteSessionEligibility({skipBundle})`. Function name is wrong in the chapter.
- Verdict: **drift** (signature differs, called function name wrong)

### Snippet 13: runAgent.ts:L761-L768 (pushApiMetricsEntry)
- File exists: YES
- Content: Matches actual source closely. Line reference is valid.
- Verdict: **verbatim**

### Snippet 14: LocalAgentTask.tsx:L43-L57 (ProgressTracker — duplicate)
- This is a repeat of Snippet 6.
- Verdict: **drift** (same issues as snippet 6)

## Summary

- Total snippets: 14
- Verbatim: 7
- Drift: 7
- Hallucinated: 0

## Uncited Sources

All 5 source files from the brief are cited:
- src/tools/AgentTool/runAgent.ts — cited
- src/tools/AgentTool/forkSubagent.ts — cited
- src/utils/forkedAgent.ts — cited
- src/tasks/RemoteAgentTask/RemoteAgentTask.tsx — cited
- src/tasks/LocalAgentTask/LocalAgentTask.tsx — cited

## Issues

1. Snippet 4: Comments omitted from RemoteAgentTaskState type
2. Snippet 5: `unregisterCleanup` field missing from LocalAgentTaskState
3. Snippet 6: Line offset wrong (L43 vs actual L41), comments omitted from ProgressTracker
4. Snippet 7: Fallback path (no tool_use blocks) omitted from buildForkedMessages
5. Snippet 9: buildChildMessage content is paraphrased, not verbatim
6. Snippet 12: checkRemoteAgentEligibility signature wrong (missing skipBundle param), called function name wrong (checkBackgroundRemoteSessionPrecondition vs checkBackgroundRemoteSessionEligibility)
7. Snippet 14: Duplicate of snippet 6
