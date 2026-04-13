# Polish Audit: Chapter 40 - MCP: Clients, Transports, and Lifecycle

## Word Count
- Body word count (excluding code/mermaid): 4378
- Range: [5100, 7500]
- **BELOW MINIMUM**: 4378 < 5100

## Forbidden Tokens
None found.

## Required Sections
All 6 sections present in correct order:
1. Overview - present
2. Data structures and contracts - present
3. Control flow - present
4. Edge cases and failure modes - present
5. Where cc diverges from the published pattern - present
6. Developer takeaways for building a long-running agent - present

## Developer Takeaways
- Word count: 577
- Required range: [150, 300]
- **OUT OF RANGE**: 577 > 300. The takeaways section is overly long, reading more like additional chapter content than a concise takeaway summary.

## Code Snippets
- Total snippets with source-path captions: 11 (minimum: 4) - PASS
- Snippets in Data structures section: 3 (minimum: 1) - PASS
- Snippets in Control flow section: 8 (minimum: 1) - PASS

## Style Issues
1. **Numbering error**: In the "Control flow" section under "Transport connection establishment", there are two items numbered "2." (one for "Disabled servers" at line 172, another for "Session expiry" at line 174). This is a numbering restart bug.

## Verdict: REVISE

Issues requiring revision:
1. Word count below minimum (4378 vs 5100 required)
2. Developer takeaways section exceeds 300-word limit (577 words)
3. Numbering error in control flow section
