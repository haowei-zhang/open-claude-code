# Consistency Audit Report - Chapter 25: Messages and the Conversation Model

## Terminology Check

The following terms from terminology.json appear in this chapter:

1. **tool** - Used correctly ("tool_use", "tool_result", "tool dispatch"). Consistent with definition.
2. **compaction** - Used correctly as "the process of reducing conversation context length by summarizing or discarding older messages." Consistent.
3. **microcompact** - Used correctly as "A lightweight compaction pass." Consistent.
4. **context collapse** - Referenced but not discussed in depth. Consistent usage.
5. **autocompact** - Referenced but not discussed in depth. Consistent usage.
6. **memory** - Used in context of memory injection and correction hints. Consistent with "persistent note stored in the memdir tiered-memory system."
7. **memdir** - Referenced indirectly through memory injection. Consistent.
8. **session** - Used correctly ("session JSONL", "session persistence"). Consistent with "single invocation from startup to shutdown, persisted as JSONL entries."
9. **harness** - Used correctly ("harness-generated messages", "harness-internal events"). Consistent with "the outer runtime layer that orchestrates the model's interactions."
10. **hook** - Used in context of hook attachments ("HookAttachment", "hook_blocking_error"). Consistent with "deterministic lifecycle event handler."
11. **permission mode** - Referenced in createUserMessage. Consistent with "named configuration governing tool-access behavior."
12. **classifier** - Used correctly ("auto-mode classifier", "classifier unavailability"). Consistent with "function that maps a shell command string to a risk band."
13. **observation masking** - Not used in this chapter. N/A.
14. **PreToolUse** / **PostToolUse** - Referenced through hook attachments. Consistent.
15. **progressive tool expansion** - Not used in this chapter. N/A.

## Term Conflicts

1. The chapter uses "auto-mode classifier" and "auto mode classifier" interchangeably. The terminology registry defines "classifier" without the "auto-mode" prefix. The hyphenated vs non-hyphenated variation is minor but should be consistent.
2. The chapter uses "tool_use" and "tool_use blocks" which is consistent with the codebase's naming convention (snake_case for API-level concepts). No conflict.

## Voice Drift

The chapter maintains the house style ("present tense, descriptive, cite-heavy") throughout. No voice drift detected. The prose is technical and factual, with appropriate use of present tense for describing code behavior.

## Proposed New Terms

1. **synthetic message** - A harness-generated message that never originates from the user or the model, used for flow control (INTERRUPT_MESSAGE, CANCEL_MESSAGE, REJECT_MESSAGE, NO_RESPONSE_REQUESTED).
2. **message normalization** - The process of splitting multi-block messages into individual single-block messages with derived UUIDs, ensuring API structural requirements are met.
3. **denial workaround guidance** - The DENIAL_WORKAROUND_GUIDANCE pattern that constrains model behavior after permission denials, allowing reasonable workarounds while preventing malicious bypasses.
4. **memory correction hint** - A just-in-time behavioral nudge appended to rejection messages when auto-memory is enabled, steering the model toward saving feedback at the moment of denial.
5. **short message ID** - A 6-character base36 hash derived from a message UUID for use in the history_snip compaction system.
