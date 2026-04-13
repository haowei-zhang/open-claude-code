# Cross-Reference Audit for Chapter 41

## Summary
Chapter 41 is required to cite HER Section 5 Pattern 9 (Progressive Tool Expansion) and Section 6.13 (Prompt Injection via MCP responses).

## Findings

### Required HER Refs
1. **§5 Pattern 9 Progressive Tool Expansion** - CITED and substantively discussed. The chapter opens with a reference to HER Pattern 9, traces the deferred loading mechanism, and connects it to the `shouldDefer` flag and `ToolSearchTool`. The "Deferred loading and ToolSearch integration" section (line 172) provides detailed discussion.

2. **§6.13 Prompt Injection via MCP responses** - CITED and substantively discussed. The "Prompt injection via MCP responses" section (line 411) directly addresses this failure mode, listing specific mitigations and acknowledging the remaining gap.

### Divergence Section
The "Where cc diverges from the published pattern" section starts at line 506. It contains 5 subsections covering:
- Passthrough permissions vs. capability-based trust
- No per-server permission scoping
- Description truncation as a safety measure
- The mcp__ prefix and tool name normalization
- Elicitation handling, large result handling

Word count of divergence section: approximately 350 words (substantive, not a one-liner).

### Issues
- None found. Both required refs are present and substantively engaged.
- Divergence section is adequate (350+ words).
- At least 2 distinct HER references are present.

## Verdict: PASS
