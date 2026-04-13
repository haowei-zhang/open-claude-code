# Consistency Audit: Chapter 15 — Search, LSP, and Code Analysis Tools

## Terminology consistency check

Checked each term in terminology.json against usage in this chapter:

1. **tool** — Used consistently with canonical definition. The chapter refers to "GrepTool", "LSPTool", "ToolSearchTool" as tools throughout. PASS.

2. **deferred tool** — Used consistently. The chapter says "defers the rest" and "deferred tool" throughout. PASS.

3. **ToolSearch** — Used as the tool name consistently. PASS.

4. **shouldDefer** — The chapter says "The ToolSearchTool and WebFetchTool both set shouldDefer: true" — this is factually wrong (ToolSearchTool does NOT set shouldDefer) but the term usage itself is consistent with the canonical definition. NOT a term conflict but a factual error (caught by accuracy auditor).

5. **harness** — Used once in takeaways: "For any progressive tool expansion system" and "the harness to inject continuation messages" — consistent. PASS.

6. **dispatch pipeline** — Not used in this chapter. N/A.

7. **query loop** — Not used. N/A.

8. **observation masking** — Not used. N/A.

9. **compaction** — Not used. N/A.

10. **permission mode** — Not used. N/A.

11. **classifier** — Not used. N/A.

12. **PreToolUse / PostToolUse** — Not used. N/A.

## Alternate name conflicts

- No "sub-agent" vs "subagent" conflicts (term not used)
- No "tool call" vs "tool use" conflicts
- No "tool use" vs "tool call" conflicts

## Voice drift

The chapter is written in present tense, descriptive style with code citations. This matches the house style ("present tense, descriptive, cite-heavy"). No voice drift detected.

## Proposed new terms

1. **progressive tool expansion** — The mechanism by which cc starts with a small set of core tools and loads additional tools on demand via ToolSearch, reducing initial prompt size and improving selection accuracy. (This is already in the registry as "deferred tool" and "ToolSearch", but "progressive tool expansion" as a compound concept is not registered.)

2. **keyword scoring** — The weighted search algorithm in ToolSearchTool that parses queries into terms and matches them against tool name parts, search hints, and descriptions with different weights (10/12 for exact name match, 5/6 for partial, 4 for hint, 2 for description).

3. **search hint** — A curated, high-signal capability phrase on a tool definition (e.g., 'search file contents with regex (ripgrep)') that scores higher than the full description in ToolSearchTool keyword matching.

4. **head-limit pagination** — The pattern in GrepTool and GlobTool where a default limit (250) caps result size, with explicit `[Showing results with pagination = limit: N]` signaling when truncation occurs, enabling the model to paginate with offset.

5. **semantic type wrapper** — A Zod schema wrapper (semanticNumber, semanticBoolean) that coerces string-typed fields from LLM JSON output into their proper types, reducing tool-call failures from type mismatches.

## Summary

- Term conflicts: 0
- Voice drift: none
- Proposed new terms: 5
