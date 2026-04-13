# Consistency Audit: Chapter 21

## Term Conflicts

1. **feature gate** vs **feature flag**: The terminology registry defines "feature gate" as the canonical term. The chapter uses "feature flag" at one point ("a compile-time feature flag") when it should use "feature gate".

## Voice Drift

No voice drift detected. Chapter uses present tense, descriptive, cite-heavy style consistent with house style.

## Proposed New Terms

1. **coordinator mode** -- An operational mode where the parent agent orchestrates worker subagents instead of implementing directly, gated by a compile-time feature gate and a runtime environment variable.
2. **scratchpad** -- A feature-gated shared directory for durable cross-worker knowledge sharing in coordinator mode, allowing workers to read and write without permission prompts.
3. **worker** -- A subagent with subagent_type: worker dispatched by the coordinator to research, implement, or verify code changes in an isolated context.
4. **synthesis** -- The coordinator's process of reading and understanding worker findings before delegating follow-up work, producing a specific implementation spec with file paths, line numbers, and expected outcomes.
5. **continue-vs-spawn decision** -- The coordinator's decision whether to continue an existing worker via SendMessage or spawn a fresh worker via AgentTool, based on context overlap between the worker's current context and the next task.

## Verdict

1 term conflict. Verdict: **revise**.
