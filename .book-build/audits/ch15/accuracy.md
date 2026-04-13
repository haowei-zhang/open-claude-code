# Accuracy Audit: Chapter 15 — Search, LSP, and Code Analysis Tools

## Citation Verification

### Snippets verified against source files

1. `src/tools/GrepTool/GrepTool.ts:L33-L89` — GrepTool inputSchema. **VERBATIM**. The chapter matches the actual code exactly. Line numbers are accurate (inputSchema starts at line 33 in the source).

2. `src/tools/GrepTool/GrepTool.ts:L144-L156` — GrepTool outputSchema. **VERBATIM**. Matches lines 144-155 in source. The `appliedOffset` field is present in the real code at line 153.

3. `src/tools/LSPTool/LSPTool.ts:L59-L86` — LSPTool inputSchema. **VERBATIM**. Matches lines 59-86 exactly.

4. `src/tools/ToolSearchTool/ToolSearchTool.ts:L21-L35` — ToolSearchTool inputSchema. **VERBATIM**. Matches lines 21-34 exactly.

5. `src/tools/ToolSearchTool/ToolSearchTool.ts:L37-L45` — ToolSearchTool outputSchema. **VERBATIM**. Matches lines 37-44 exactly.

6. `src/tools/GrepTool/GrepTool.ts:L529-L556` — Sort logic. **VERBATIM**. Matches lines 529-553 exactly.

7. `src/tools/GrepTool/GrepTool.ts:L110-L128` — applyHeadLimit function. **VERBATIM**. Matches lines 110-128 exactly.

8. `src/tools/ToolSearchTool/ToolSearchTool.ts` (memoization) — No line-range caption. This is a prose description block, not a verbatim code snippet. **PASS** as a descriptive reference.

### Factual claims verification

- "GrepTool uses ripgrep" — Confirmed by `import { ripGrep }` in source.
- "LSPTool supports eight operations" — Confirmed by the enum in source (goToDefinition, findReferences, hover, documentSymbol, workspaceSymbol, goToImplementation, prepareCallHierarchy, incomingCalls, outgoingCalls). Note: this is actually NINE operations, not eight. The chapter says "eight operations" in the Data structures section but lists nine in the Control flow section. This is a minor factual inconsistency.
- "ToolSearchTool keyword scoring weights: exact name-part 10, MCP 12, partial 5/6, hint 4, description 2" — Confirmed by source code scoring logic.
- "10 MB cap for LSPTool" — Confirmed by `MAX_LSP_FILE_SIZE_BYTES = 10_000_000` in source.
- "maxResultSizeChars: 20_000 for GrepTool" — Confirmed in source.
- "shouldDefer: true for LSPTool and ToolSearchTool" — Confirmed: LSPTool has `shouldDefer: true`, but ToolSearchTool does NOT have `shouldDefer: true` — it is the search tool itself, not deferred. The chapter states "The ToolSearchTool and WebFetchTool both set shouldDefer: true" — this is **INCORRECT** for ToolSearchTool. ToolSearchTool cannot defer itself; it is the mechanism for finding deferred tools. The `isDeferredTool` function in prompt.ts explicitly excludes ToolSearchTool: `if (tool.name === TOOL_SEARCH_TOOL_NAME) return false`.
- "GrepTool sets maxResultSizeChars: 20_000" — Confirmed in source.
- "semanticNumber and semanticBoolean wrappers" — Confirmed by imports and usage in GrepTool source.
- "MCP tool names parsed via mcp__server__action format split on __ and _" — Confirmed by `parseToolName` in source.

### Issues found

1. **Wrong claim (ch line ~329)**: "The ToolSearchTool and WebFetchTool both set shouldDefer: true" — ToolSearchTool does NOT set shouldDefer. It is excluded from deferral by `isDeferredTool()`.

2. **Factual inconsistency**: Chapter states "eight operations" for LSPTool but lists nine operations (goToDefinition, findReferences, hover, documentSymbol, workspaceSymbol, goToImplementation, prepareCallHierarchy, incomingCalls, outgoingCalls).

### Uncited source files

The following source files from the brief are not cited in the chapter:
- `src/tools/LSPTool/formatters.ts` — Mentioned by name but not cited with line numbers
- `src/tools/LSPTool/schemas.ts` — Not cited
- `src/tools/LSPTool/symbolContext.ts` — Not cited
- `src/tools/LSPTool/prompt.ts` — Not cited
- `src/tools/LSPTool/UI.tsx` — Not cited
- `src/tools/ToolSearchTool/constants.ts` — Not cited
- `src/tools/ToolSearchTool/prompt.ts` — Not cited
- `src/services/lsp/LSPClient.ts` — Not cited
- `src/services/lsp/LSPServerInstance.ts` — Not cited
- `src/services/lsp/LSPServerManager.ts` — Not cited
- `src/services/lsp/LSPDiagnosticRegistry.ts` — Not cited
- `src/services/lsp/manager.ts` — Not cited
- `src/services/lsp/config.ts` — Not cited
- `src/services/lsp/passiveFeedback.ts` — Not cited

These are informational. The chapter focuses on the tool implementations and schemas, not the LSP infrastructure.

## Summary

- 7 verbatim snippets with source-path captions, 1 prose description snippet
- 1 incorrect factual claim (ToolSearchTool shouldDefer)
- 1 minor factual inconsistency (8 vs 9 LSP operations)
- All snippets are verbatim matches
- No hallucinated snippets
