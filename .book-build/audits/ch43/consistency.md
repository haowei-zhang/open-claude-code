# Consistency Audit: Chapter 43

## Verdict: revise

## Term Conflicts

1. **"sub-agent" vs "subagent"**: The chapter uses "subagent" consistently, matching the terminology registry. No conflict.

2. **"tool call" vs "tool use"**: The chapter uses "tool call" in some places (e.g., "tool call" in the context of bash mode wrapping input in a Bash tool call). The registry defines "tool" and "dispatch pipeline" but does not have separate entries for "tool call" vs "tool use". No conflict with registered terms.

3. **"REPL"**: Used consistently throughout. Not in the terminology registry, but is a well-known abbreviation.

4. **"typeahead"**: Used consistently. Not in the terminology registry.

5. **"progressive disclosure"**: The chapter uses "progressive disclosure" in the divergence section when comparing to HER Pattern 9 (Progressive Tool Expansion). The registry has "progressive tool expansion" and "deferred tool". The chapter correctly differentiates between progressive tool expansion (HER Pattern 9) and progressive disclosure (the UI pattern). No conflict.

6. **"feature gate"**: Used correctly per registry definition at L256 ("gated by the VOICE_MODE feature flag") and L449.

7. **"early input buffer"**: The term is not used in this chapter but is relevant to REPL startup. Not a conflict.

8. **"scroll drain"**: Not mentioned in this chapter. Not a conflict.

## Voice Drift

The chapter maintains present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **"typeahead engine"**: The multi-source suggestion aggregation system in useTypeahead that merges slash commands, file paths, shell completions, Slack channels, and agent names into a unified ranked list with debouncing and stale-result filtering.

2. **"ghost text"**: An inline completion hint rendered as dimmed text after the cursor position, providing the most likely suggestion without requiring the user to open a suggestion menu.

3. **"hold-to-talk"**: A voice input protocol where the user presses and holds a key to record audio, releasing it to send for transcription, with auto-repeat key event handling and release timeout detection.

4. **"keybinding context"**: A string identifier (e.g., 'Global', 'Transcript', 'Prompt') that determines when a keyboard handler is active, enabling context-aware key routing where the most specific context takes priority.

## Summary

- term_conflicts: 0
- voice_drift: null
- proposed_new_terms: 4 terms proposed
