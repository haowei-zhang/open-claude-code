# Accuracy Audit: Chapter 40 - MCP: Clients, Transports, and Lifecycle

## Citation Verification

All 6 source files from the brief are cited:
- `src/services/mcp/client.ts` - cited multiple times
- `src/services/mcp/config.ts` - cited multiple times
- `src/services/mcp/auth.ts` - cited multiple times
- `src/services/mcp/types.ts` - cited multiple times
- `src/services/mcp/MCPConnectionManager.tsx` - cited once
- `src/entrypoints/mcp.ts` - cited once

## Snippet Verification

| # | Citation | Verdict | Notes |
|---|----------|---------|-------|
| 1 | types.ts:L180-L227 | drift | serverInfo field inline vs multiline |
| 2 | types.ts:L23-L26 | verbatim | Exact match |
| 3 | types.ts:L29-L35 | verbatim | Exact match |
| 4 | config.ts:L69-L81 | drift | Brace placement differences |
| 5 | config.ts:L88-L131 | drift | Comment and brace formatting |
| 6 | config.ts:L202-L212 | drift | if-else brace formatting |
| 7 | client.ts:L177-L186 | **hallucinated** | Class names obfuscated in source, not matching |
| 8 | client.ts:L188-L206 | drift | Brace and formatting compression |
| 9 | auth.ts:L147-L151 | verbatim | Exact match |
| 10 | mcp.ts:L35-L57 | **hallucinated** | Simplified code not matching actual source at those lines |
| 11 | MCPConnectionManager.tsx:L7-L15 | drift | Semicolon and formatting differences |

## Critical Issues

1. **Snippet 7 hallucinated**: client.ts:L177-L186 shows `McpToolCallError extends TelemetrySafeError` but the source has obfuscated suffixed names.
2. **Snippet 10 hallucinated**: mcp.ts:L35-L57 shows a simplified version missing cache setup lines that exist in the source.

## Verdict: REWRITE

Two hallucinated snippets require correction. The snippet content does not match the actual source code at the cited line ranges.
