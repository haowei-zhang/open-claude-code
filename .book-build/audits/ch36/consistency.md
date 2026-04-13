# Consistency Audit: Chapter 36 - The Hook Schema and Lifecycle Events

## Terminology Check

### Registered terms used in this chapter:
- **hook** - Used consistently with canonical definition ("deterministic lifecycle event handler"). PASS.
- **PreToolUse** - Used correctly as a hook event name. PASS.
- **PostToolUse** - Used correctly as a hook event name. PASS.
- **lazySchema** - Used correctly ("wrapper function that defers Zod schema evaluation until first call"). PASS.
- **subagent** - Used correctly. PASS.
- **permission mode** - Not directly used. PASS.
- **classifier** - Referenced in PermissionDenied context. PASS.
- **stop hook** - Used correctly. PASS.
- **compaction** - Referenced in PreCompact/PostCompact context. PASS.
- **SSRF guard** - Referenced in passing. PASS.
- **dispatch pipeline** - Not used. N/A.
- **settings cascade** - Referenced conceptually but not by name. PASS.

### Term Conflicts
- None detected. All registered terms are used with their canonical definitions.

### Voice Drift
- Chapter is written in present tense, descriptive, cite-heavy style. Consistent with house style.
- No voice drift detected.

### Alternate Names
- "sub-agent" not used (correctly uses "subagent")
- "tool use" vs "tool call" - Chapter uses both "tool call" and "tool use". The terminology registry defines "tool" but not a preference for "tool call" vs "tool use". Minor inconsistency but not a registered conflict.

## Proposed New Terms
1. **async rewake** - A flag on command hooks that causes the hook to run in the background and wake the model if it exits with code 2, enabling blocking-error detection for long-running background tasks.
2. **hook matcher** - A grouping construct that associates one or more hooks with a matcher string, where the matcher is event-specific (tool names for PreToolUse/PostToolUse, notification types for Notification, etc.).
3. **exit code semantics** - The three patterns (block-on-2, show-on-2, output-as-input) that define how a hook's exit code is interpreted for each lifecycle event.
4. **once-hook** - A hook with `once: true` that is removed after its first execution, useful for one-time setup commands.
5. **env var interpolation** - The mechanism in HTTP hooks that replaces `$VAR_NAME` or `${VAR_NAME}` patterns in header values with environment variable values, gated by an `allowedEnvVars` allowlist.
