# Cross-Reference Audit: Chapter 40 - MCP: Clients, Transports, and Lifecycle

## Required HER References

| HER Ref | Found? | Location in Chapter |
|---------|--------|---------------------|
| §7.2 MCP Servers | Yes | Line 7, Line 287, Line 333-338 |
| §12.4 Supply Chain via MCP | Yes | Line 7, Line 339 |
| §6.15 Data Leakage | Yes | Line 323, Line 347 |

All 3 required HER references are cited in the chapter.

## Divergence Section

The "Where cc diverges from the published pattern" section (lines 331-348) contains approximately 385 words and is substantive. It covers three specific divergences:

1. No sandboxing for stdio MCP servers
2. MCP skill shell execution bypass
3. MCP resource access is not sandboxed

Each divergence is explained with concrete code references and security implications. The section exceeds the 150-word minimum.

## HER Engagement Quality

The chapter correctly engages with HER concepts:
- Pattern 9 (Progressive Tool Expansion) referenced for deferred MCP tool loading
- Section 7.2 "too many tools is bad" warning addressed via description capping
- Section 12.4 supply chain threat model applied to MCP server trust
- Section 6.15 data leakage risk mapped to MCP resource access
- Section 8.1 observation masking applied to MCP output truncation

## Verdict: PASS
