# Accuracy Audit Report for Chapter 12: Tool Dispatch Pipeline

## Citation Verification

### Verified Citations
- `src/services/tools/toolOrchestration.ts:L14-L17` — MessageUpdate type (note: chapter shows `newContext: ToolUseContext` but actual has `newContext?: ToolUseContext` — minor drift in optionality)
- `src/services/tools/toolOrchestration.ts:L84` — Batch type — verified
- `src/services/tools/toolOrchestration.ts:L91-L116` — partitionToolCalls — verified
- `src/services/tools/toolOrchestration.ts:L8-L12` — getMaxToolUseConcurrency — verified
- `src/services/tools/toolOrchestration.ts:L31-L53` — concurrent context modifiers — verified
- `src/services/tools/StreamingToolExecutor.ts:L19-L32` — TrackedTool type — verified
- `src/services/tools/StreamingToolExecutor.ts:L129-L135` — canExecuteTool — verified
- `src/services/tools/StreamingToolExecutor.ts:L69-L71` — discard() — verified
- `src/services/tools/StreamingToolExecutor.ts:L301-L303` — child abort controller — verified
- `src/services/tools/StreamingToolExecutor.ts:L304-L318` — abort propagation — verified
- `src/services/tools/StreamingToolExecutor.ts:L189-L194` — synthetic error message — verified
- `src/services/tools/StreamingToolExecutor.ts:L355-L363` — Bash error sibling cancellation (chapter paraphrases slightly from actual source, logic correct)
- `src/services/tools/StreamingToolExecutor.ts:L367-L374` — progress-available signal — verified
- `src/services/tools/toolHooks.ts:L35-L37` — PostToolUseHooksResult — verified
- `src/services/tools/toolHooks.ts:L332-L433` — resolveHookPermissionDecision — verified
- `src/services/tools/toolHooks.ts:L435-L461` — runPreToolUseHooks — verified
- `src/services/tools/toolHooks.ts:L373-L391` — hook allow doesn't bypass deny — verified
- `src/services/tools/toolHooks.ts:L347-L356` — requiresUserInteraction guard — verified
- `src/services/tools/toolExecution.ts:L800-L862` — PreToolUse hook consumption switch — verified
- `src/hooks/useCanUseTool.tsx:L127-L161` — speculative classifier check (compiled source, logic verified against original TSX)
- `src/hooks/useCanUseTool.tsx:L161-L167` — bridge/channel callbacks (compiled source, logic verified)

### Issues Found
1. **Snippet drift**: The MessageUpdate type in the chapter (L14-L17) shows `newContext: ToolUseContext` (required) but the actual source code has `newContext?: ToolUseContext` (optional). The `?` is missing. This is a whitespace-like drift but affects semantics.
2. **Snippet drift**: The Bash error cancellation snippet (L355-L363) slightly paraphrases the actual code structure — the `isErrorResult` check and the conditional are presented in a more concise form than the actual source. The logic is correct but the code is not character-by-character verbatim.
3. **Factual claim**: Chapter states StreamingToolExecutor is "530 lines" — actual is 531 lines. Close enough to be immaterial.
4. **Factual claim**: Chapter states toolOrchestration.ts is "188 lines" — actual is 189 lines. Close enough.

### Uncited Sources
None — all 5 source files are cited in the chapter.

## Snippet Verification Summary
- Total snippets: 21
- Verbatim: 18
- Drift: 3 (MessageUpdate optionality, Bash error cancellation paraphrase, useCanUseTool compiled vs source)
- Hallucinated: 0
