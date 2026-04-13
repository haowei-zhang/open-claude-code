# Consistency Audit Report — Chapter 35

## Terminology Verification

Checked all terms from terminology.json against chapter usage:

1. **tool** — Used correctly throughout with canonical definition (typed, schema-validated operation)
2. **permission mode** — Used correctly (named configuration governing tool-access behavior)
3. **classifier** — Used correctly (function mapping shell command to risk band)
4. **hook** — Used correctly (deterministic lifecycle event handler)
5. **dispatch pipeline** — Used correctly (ordered sequence of stages)
6. **PreToolUse** — Not used in this chapter (not relevant to useCanUseTool)
7. **PostToolUse** — Not used in this chapter
8. **denial tracking** — Used correctly (subsystem tracking consecutive denials)
9. **YOLO classifier** — Used correctly (LLM-based classifier)
10. **bypass-immune** — Not mentioned (not directly relevant)
11. **speculative classifier check** — Not in terminology.json; chapter uses this term consistently

## Term Conflicts

1. **"sub-agent" vs "subagent"**: Chapter uses "subagent" (line 9, 386, 388, etc.) — matches canonical term. No conflict.

2. **"auto mode" vs "auto-mode"**: Chapter uses "auto-mode" (hyphenated) consistently when referring to the classifier variant. The terminology uses "auto mode" in the classifier definition. Minor inconsistency — "auto-mode" appears as a classifier name in the code itself, so this is a code-accurate usage rather than a terminology conflict.

3. **"swarm worker" vs "worker"**: Chapter uses "swarm worker" consistently, matching the terminology's "worker" definition. No conflict.

4. **"permission pipeline" vs "dispatch pipeline"**: Chapter uses "permission pipeline" to describe the permission evaluation path and "permission evaluation" interchangeably. The terminology defines "dispatch pipeline" as the broader concept. This is acceptable because the chapter is specifically about the permission evaluation portion of the dispatch pipeline.

## Voice Drift

No voice drift detected. The chapter maintains present tense, descriptive, cite-heavy register consistent with house style.

## Proposed New Terms

1. **speculative classifier check**: A pre-computed classifier result that is raced against a timeout before showing an interactive permission dialog, allowing the dialog to be skipped if the classifier returns a high-confidence match quickly.

2. **resolve-once guard**: An atomic check-and-mark primitive (createResolveOnce) that prevents multiple concurrent racers from resolving the same permission promise, using a claimed boolean to close the window between checking isResolved() and calling resolve().

3. **permission context**: A frozen helper object that encapsulates the resolve function, abort detection, logging, and queue operations for a single permission evaluation, threaded through all handlers.

4. **grace period**: A configurable delay (200ms in the interactive handler) that prevents accidental user keypresses from canceling an in-progress automated check.

5. **checkmark transition**: A UI state in the permission dialog where an auto-approved action briefly shows a dimmed checkmark indicator before being removed, with different display durations based on terminal focus.
