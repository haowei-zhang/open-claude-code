# Gaps Audit: Chapter 15 — Search, LSP, and Code Analysis Tools

## 1. Overview vs synopsis match

Brief synopsis: "`GrepTool`, `LSPTool`, `ToolSearchTool`. Progressive tool disclosure and why it matters for large catalogs."

Chapter overview: Covers GrepTool, LSPTool, and ToolSearchTool. Discusses progressive tool expansion (disclosure) and its importance. MATCH.

## 2. Source file citation check

Brief source files:
- `src/tools/LSPTool/LSPTool.ts` — CITED (schema snippet, call flow discussion)
- `src/tools/LSPTool/formatters.ts` — Mentioned by name ("dedicated formatters") but not cited with line numbers. PARTIALLY CITED.
- `src/tools/LSPTool/schemas.ts` — NOT CITED
- `src/tools/LSPTool/symbolContext.ts` — NOT CITED
- `src/tools/LSPTool/prompt.ts` — NOT CITED
- `src/tools/LSPTool/UI.tsx` — NOT CITED
- `src/tools/ToolSearchTool/ToolSearchTool.ts` — CITED (schema snippets, search algorithm)
- `src/tools/ToolSearchTool/constants.ts` — NOT CITED
- `src/tools/ToolSearchTool/prompt.ts` — NOT CITED (but its isDeferredTool function is discussed)
- `src/services/lsp/LSPClient.ts` — NOT CITED
- `src/services/lsp/LSPServerInstance.ts` — NOT CITED
- `src/services/lsp/LSPServerManager.ts` — NOT CITED
- `src/services/lsp/LSPDiagnosticRegistry.ts` — NOT CITED
- `src/services/lsp/manager.ts` — NOT CITED
- `src/services/lsp/config.ts` — NOT CITED
- `src/services/lsp/passiveFeedback.ts` — NOT CITED

Note: `src/tools/GrepTool/GrepTool.ts` is used heavily (3 code snippets) but is NOT listed in the brief's source_files. The brief also mentions `src/tools/GrepTool/` files in chapter 13's source list. The GrepTool is covered here as part of the chapter's topic but was not in the brief's source list.

Uncited brief files: 13 out of 16 source files are not cited with line numbers.

## 3. Mandated diagrams check

Brief requires:
- (a) sequenceDiagram of ToolSearch -> deferred tool -> first call — PRESENT (ToolSearch keyword search sequence diagram)
- (b) classDiagram of LSP client lifecycle — NOT PRESENT. The chapter has a classDiagram of ToolSearchTool/ParsedToolName/DeferredTool instead. The LSP client lifecycle is described in prose in the "Deep dive" section but without a class diagram.

Missing diagrams: 1

## 4. Minimum counts

- Citation count: 7 (matches manifest) — meets minimum of 6. PASS.
- Diagram count: 3 — meets minimum of 2. PASS.
- Snippet count: 8 — meets minimum of 4. PASS.

## 5. Top 3 source files without snippets

The top 3 source files by centrality to the chapter's topic:
1. `src/tools/GrepTool/GrepTool.ts` — HAS 3 snippets (not in brief but central)
2. `src/tools/LSPTool/LSPTool.ts` — HAS 1 snippet
3. `src/tools/ToolSearchTool/ToolSearchTool.ts` — HAS 2 snippets

All top 3 files have at least one snippet. PASS.

## 6. Uncovered topics

1. **LSP server manager architecture**: The chapter mentions the LSP server manager and its lifecycle phases but does not detail the manager's architecture (how it routes requests, manages multiple servers, handles startup/shutdown). The `LSPServerManager` and `LSPClient` files are in the brief's source list but are not covered.

2. **LSP diagnostic registry**: The `LSPDiagnosticRegistry.ts` and `passiveFeedback.ts` are in the brief's source list. The chapter does not discuss how LSP diagnostics (errors, warnings) from language servers are captured and surfaced to the model. This is a gap for a chapter titled "Search, LSP, and Code Analysis Tools."

3. **GrepTool's UI rendering**: The GrepTool's `UI.tsx` is not in the brief's source list for this chapter (it's in chapter 13's list), so this is not a gap per the brief.

4. **ToolSearchTool's prompt.ts and isDeferredTool**: The `prompt.ts` file contains the `isDeferredTool` function and the prompt text. The chapter references the concept of deferral but does not show the `isDeferredTool` function or the prompt text. This is a minor gap since the deferral logic is relevant to understanding progressive tool expansion.

## Summary

- Uncited brief files: 13
- Missing diagrams: 1 (LSP client lifecycle class diagram)
- Uncovered topics: 2-3
- Top files without snippets: 0
