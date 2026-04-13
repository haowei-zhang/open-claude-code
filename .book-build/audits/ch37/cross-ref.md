# Cross-Reference Audit: Chapter 37

## Required HER References

1. **§5 Pattern 12**: The chapter cites Pattern 12 (Deterministic Lifecycle Hooks) extensively in the Overview and the "Where cc diverges" section. It explicitly states "The hook execution pipeline is the runtime arm of HER Pattern 12 (Deterministic Lifecycle Hooks)." Found and substantive.

2. **§12.4 Supply Chain Attacks via MCP/Skills/Hooks**: The chapter references this in the "Where cc diverges" section, specifically regarding SSRF defense for HTTP hooks and environment variable interpolation controls as defense-in-depth against supply chain attacks. Also referenced in the edge cases section. Found and substantive.

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive with 6 numbered divergences covering: multiple execution modes, SSRF defense, env var interpolation controls, function hooks, session-scoped hook registry, and event broadcasting. The section is approximately 350+ words.

## Issues
None — both required HER refs are present and substantively engaged.

## Verdict: pass
