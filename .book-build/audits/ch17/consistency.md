# Consistency Audit — Chapter 17

## Term Conflicts

No term conflicts found. The chapter uses canonical terminology throughout:
- "subagent" (not "sub-agent")
- "tool" (not alternate forms)
- "permission mode" (canonical)
- "compaction" (canonical)
- "feature gate" (canonical, used correctly in the Brief section)
- "deferred tool" (canonical, used correctly for AskUserQuestion's shouldDefer)
- "dispatch pipeline" (canonical, used in reference to tool execution)

The phrase "tool call" appears once but the registry does not define "tool use" as a canonical term, so this is not a conflict.

## Voice Drift

None detected. The chapter is written in present tense, descriptive style, cite-heavy (24+ citations), consistent with the house style.

## Proposed New Terms

1. **verification nudge** — The advisory signal in TodoWrite that fires when the main-thread agent completes 3+ tasks without a verification step, appending a NOTE block to the tool result reminding the model to spawn the verification subagent.

2. **entitlement-activation gate** — The two-layer availability pattern where a tool requires both entitlement (enrolled in experiment/feature flag) and activation (explicit user opt-in), as implemented by Brief's isBriefEntitled() + isBriefEnabled().

3. **structured message protocol** — A discriminated-union message schema for inter-agent communication supporting typed lifecycle events (shutdown_request, shutdown_response, plan_approval_response) with request IDs for correlation.

4. **safety-check decision reason** — A permission decision reason type (decisionReason.type === 'safetyCheck') that makes a tool immune to bypassPermissions and auto-mode classifiers, used for cross-machine communication in SendMessage.

5. **auto-resume on send** — The pattern where SendMessage attempts resumeAgentBackground() when targeting a stopped agent, restarting it with the incoming message as prompt.
