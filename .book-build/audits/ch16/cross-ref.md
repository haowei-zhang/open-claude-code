# Cross-Reference Audit Report for Chapter 16

## Required HER References

1. **§6.13 Prompt Injection** - FOUND. Cited in the Overview, Edge cases section (line 249), and the Divergence section (lines 255, 261, 263). Substantive engagement with the failure mode, including specific discussion of indirect prompt injection via web content, the CYBER_RISK_MITIGATION_REMINDER gap, and the HTML-to-markdown limitation.

2. **§6.12 Hallucinated Tool Calls** - FOUND. Cited in the Overview and Divergence section (line 263). Discusses how Zod schema validation catches structurally invalid parameters but cannot detect semantically wrong parameters (e.g., valid-looking URLs pointing to malicious servers).

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive. It covers 6 divergence points with substantial detail:
1. Least-privilege tool permissions
2. SSRF defense
3. No web-content sanitization beyond HTML-to-markdown
4. No hallucinated-tool-call detection
5. WebSearchTool is server-delegated
6. No rate limiting on WebFetchTool

Word count of divergence section: ~350 words (exceeds 150-word minimum).

## Issues

- No issues found. Both required HER refs are present and substantively engaged.
- Divergence section is well-developed.
