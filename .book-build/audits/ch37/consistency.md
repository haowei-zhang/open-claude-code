# Consistency Audit: Chapter 37

## Term Conflicts

1. **"subagent" vs "sub-agent"**: The chapter uses "subagents" on line 9 and elsewhere consistently. The terminology registry defines "subagent" (no hyphen). No conflict found — usage is consistent with registry.

2. **"hook"**: Used throughout with the canonical definition "A deterministic lifecycle event handler (command, prompt, HTTP, agent, or function type) triggered at PreToolUse, PostToolUse, or other defined points." Consistent.

3. **"SSRF guard"**: Used consistently with the registry definition "The dns.lookup-compatible function in src/utils/hooks/ssrfGuard.ts that blocks connections to private, link-local, and CGNAT address ranges."

4. **"stop hook"**: Used on line 265 ("stop hooks — verification that the agent has actually completed its assigned task"). The registry defines "stop hook" as "A user-defined hook evaluated at the Stop lifecycle event." Consistent.

5. **"once-hook"**: The chapter references `once: true` hooks on line 309 and 333. The registry defines "once-hook" as "A hook with `once: true` that is removed after its first execution." Consistent.

6. **"exit code semantics"**: Referenced implicitly in the command hook section. The registry definition is consistent with the chapter's description.

7. **"async rewake"**: Referenced in the command hook section ("async hooks via the AsyncHookJSONOutput schema"). The registry defines this as a flag on command hooks. Consistent.

8. **"env var interpolation"**: Used in HTTP hook section. Matches the registry definition.

9. **"hook matcher"**: Referenced in skill hook registration section. Matches registry definition.

10. **"session"**: Used consistently with registry definition.

## Voice Drift
The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **"combined abort signal"**: The pattern of merging a parent abort signal with a timeout signal via `createCombinedAbortSignal`, used across all hook execution paths to enforce timeout boundaries. Distinct from a simple AbortController because it combines two independent abort sources.

2. **"three-outcome model"**: The hook result contract where every hook execution produces one of three outcomes: success, blocking, or non_blocking_error (plus cancelled for external interruption). This is a core invariant of the hook system.

3. **"session hook registry"**: The Map<string, SessionStore> structure keyed by session ID that stores ephemeral, in-memory hooks (both command and function type) for the duration of a session.

4. **"structured output enforcement"**: The pattern where a PostToolUse function hook checks whether the agent has called StructuredOutputTool and, if not, blocks the response and prompts the agent to use it. Used by agent hooks to guarantee structured responses.

5. **"hook event broadcasting"**: The lightweight pub/sub system in hookEvents.ts that broadcasts hook execution progress to SDK consumers and the UI, with a pending-events buffer for late-registered handlers.
