# Consistency Audit: Chapter 30 — AppState: The Redux-like Store

## Terminology Conflicts

Scanning chapter for terms from terminology.json:

1. "tool" — used correctly per canonical definition (typed, schema-validated operation)
2. "subagent" — used correctly (agent spawned by parent via AgentTool)
3. "session" — used correctly (single invocation from startup to shutdown)
4. "harness" — used correctly (outer runtime layer)
5. "compaction" — referenced implicitly; no conflict
6. "permission mode" — used correctly (named configuration governing tool-access behavior)
7. "query loop" — not used in this chapter
8. "store" — used as a general term (the Store<T> type), which is consistent with the chapter's scope

No term conflicts found. All registered terms are used with their canonical definitions.

## Voice Drift

The chapter is written in present tense, descriptive style with heavy citation, consistent with the house style. No voice drift detected.

## Proposed New Terms

1. "structural sharing" — The pattern where setState creates a new root object while sharing unchanged sub-trees by reference, enabling Object.is identity checks for re-render skipping.
2. "onChange diff" — The centralized diff pattern in onChangeAppState that compares old and new state to trigger side effects, ensuring any setState call automatically propagates changes.
3. "selector discipline" — The practice of ensuring useAppState selectors return stable references (existing sub-object references) rather than creating new objects, to prevent infinite re-renders.

## Verdict

Zero term conflicts, no voice drift. Pass.
