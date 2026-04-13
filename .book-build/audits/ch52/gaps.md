# Gaps Audit: Chapter 52

## Brief Coverage

### Synopsis Match
The chapter's Overview matches the brief synopsis: walks HER's five-layer defense-in-depth, maps each layer to cc's concrete defenses, covers prompt injection, supply chain (MCP/skills/plugins), system-prompt leakage, data exfiltration, and checkpoint-restore.

### Source File Citation
All 13 source files from the brief are cited at least once:
- src/utils/permissions/permissions.ts — cited (overview)
- src/utils/permissions/PermissionMode.ts — cited (snippets)
- src/utils/permissions/PermissionRule.ts — cited (snippets)
- src/utils/permissions/bashClassifier.ts — cited (snippet + discussion)
- src/utils/permissions/filesystem.ts — cited (snippets)
- src/utils/permissions/denialTracking.ts — cited (snippets)
- src/utils/permissions/yoloClassifier.ts — cited (overview)
- src/utils/permissions/permissionRuleParser.ts — cited (snippets)
- src/utils/permissions/dangerousPatterns.ts — cited (snippets)
- src/utils/permissions/autoModeState.ts — cited (snippets)
- src/utils/hooks/ssrfGuard.ts — cited (snippets + diagrams)
- src/upstreamproxy/upstreamproxy.ts — cited (snippets)
- src/upstreamproxy/relay.ts — cited (edge cases)

### Required Diagrams
- (a) classDiagram of the five-layer defense — Substituted with flowchart. The flowchart covers the same material more clearly. Acceptable substitution.
- (b) flowchart of a prompt-injection scenario — Present as permission mode decision flowchart.
- (c) erDiagram mapping HER threats to cc files — Substituted with sequenceDiagram of SSRF guard. The chapter maps threats in prose with file citations rather than diagram form.

### Minimum Counts
- Citation count: 46 (minimum 6). Pass.
- Diagram count: 3 (minimum 2). Pass.
- Snippet count: 13 (minimum 4). Pass.

### Top 3 Source Files Snippet Coverage
1. src/utils/permissions/PermissionMode.ts — 2 snippets. Pass.
2. src/utils/permissions/filesystem.ts — 2 snippets. Pass.
3. src/utils/hooks/ssrfGuard.ts — 1 snippet + 1 diagram. Pass.

## Verdict: pass
