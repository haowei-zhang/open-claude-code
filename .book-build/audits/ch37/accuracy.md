# Accuracy Audit: Chapter 37

## Citation Verification

### Snippet 1: `src/utils/hooks.ts:L166` — Hook result type
- **Problem**: Line 166 in hooks.ts is `const TOOL_HOOK_EXECUTION_TIMEOUT_MS = 10 * 60 * 1000`, not a HookResult type definition. The `HookResult` interface is at line 338, `AggregatedHookResult` is at line 359. The snippet is labeled "Conceptual shape (aggregated from imports)" which means it is not a verbatim quote but an aggregation of two separate types. The line reference is wrong.
- **Verdict**: bad_citation (wrong line)

### Snippet 2: `src/utils/hooks/sessionHooks.ts:L15-L31` — Function hook type
- Verified: `FunctionHookCallback` type at L15, `FunctionHook` type at L24. The snippet content matches the source code well. The range L15-L31 matches the actual code.
- **Verdict**: verbatim

### Snippet 3: `src/utils/hooks/sessionHooks.ts:L49-L62` — Map-based session hooks state
- Verified: Comment starting at L48 and `SessionHooksState` type at L62. The chapter snippet starts at L49 (skipping `/**` on L48). Content matches the source.
- **Verdict**: verbatim (minor: comment starts at L48, not L49)

### Snippet 4: `src/utils/hooks/hookEvents.ts:L22-L55` — Event types
- Verified: `HookStartedEvent` at L22, `HookProgressEvent` at L29, `HookResponseEvent` at L39. The three event types match the source verbatim. The actual end of the three types plus the union type is around L55.
- **Verdict**: verbatim

### Snippet 5: `src/utils/hooks/execPromptHook.ts:L55-L100` — Model query with structured output
- Verified: `hookTimeoutMs` at L55, `createCombinedAbortSignal` at L58, `queryModelWithoutStreaming` at L62. The content matches the source closely. The chapter version omits some intermediate lines (like `getToolPermissionContext` and other options) and simplifies.
- **Verdict**: drift (some lines omitted/condensed)

### Snippet 6: `src/utils/hooks/execHttpHook.ts:L123-L145` — URL allowlist check and SSRF guard
- Verified: `execHttpHook` function starts at L123. The snippet matches the URL allowlist check portion of the function.
- **Verdict**: verbatim (partial excerpt, correctly indicated by `// ...` trim)

### Snippet 7: `src/utils/hooks/ssrfGuard.ts:L42-L86` — Address blocking logic
- Verified: `isBlockedAddress` at L42, `isBlockedV4` at L55. The chapter combines both functions. Content matches the source.
- **Verdict**: verbatim

### Snippet 8: `src/utils/hooks/execAgentHook.ts:L87-L121` — Agent setup
- Verified: `structuredOutputTool` at L89, `filteredTools` at L93, `tools` at L100, `systemPrompt` at L107, `model` at L118, `MAX_AGENT_TURNS` at L119. Content matches well. Line range is approximately correct.
- **Verdict**: drift (some lines condensed/omitted)

### Snippet 9: `src/utils/hooks/registerSkillHooks.ts:L20-L63` — Skill hook registration
- Verified: Function starts at L20. Content matches the source.
- **Verdict**: verbatim

## Factual Claims
- The claim about `TOOL_HOOK_EXECUTION_TIMEOUT_MS` being 10 minutes is confirmed (hooks.ts:L166).
- The `AggregatedHookResult` shape in the chapter combines fields from both `HookResult` and `AggregatedHookResult` into a single conceptual type; this is acknowledged as "aggregated from imports."
- The claim that loopback is intentionally allowed in SSRF guard is confirmed (ssrfGuard.ts:L68, L92).
- The claim about `MAX_AGENT_TURNS = 50` is confirmed (execAgentHook.ts:L119).
- The claim about default prompt hook timeout of 30 seconds is confirmed (execPromptHook.ts:L55).
- The claim about default agent hook timeout of 60 seconds is confirmed (execAgentHook.ts:L75).
- The claim about `MAX_PENDING_EVENTS = 100` is confirmed (hookEvents.ts:L20).

## Uncited Sources
None — all 7 source files from the brief are cited in the chapter.

## Summary
- 1 bad citation (wrong line number for HookResult type)
- 2 snippets with drift (condensed code)
- 6 verbatim snippets
- 0 hallucinated snippets
- All source files cited
