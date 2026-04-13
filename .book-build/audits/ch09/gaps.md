# Gaps Audit: Chapter 9 — System Prompts and Prompt Assembly

## Overview vs Synopsis Match
- Synopsis: "`src/constants/prompts.ts` (~900 LOC) and how cc composes the final system prompt per query from base, environment, tools, skills, memories, hooks, effort, plan mode."
- Chapter covers: prompts.ts, CLAUDE.md loading, context analysis, context window configuration.
- **Gap**: The synopsis mentions "tools, skills, memories, hooks, effort, plan mode" as prompt assembly inputs. The chapter discusses tools, skills, and memories in the prompt assembly layers diagram, but does not deeply discuss how "effort" and "plan mode" affect the system prompt. The "effort" topic is assigned to Chapter 10, so this is expected, but the chapter does not explicitly acknowledge this handoff.

## Uncited Brief Source Files
Brief source files:
1. `src/constants/prompts.ts` — **cited** (multiple times)
2. `src/utils/analyzeContext.ts` — **cited** textually (analyzeContextUsage function discussed) but not with a formal `src/...:Lnnn` citation
3. `src/utils/claudemd.ts` — **cited** (multiple times)
4. `src/utils/context.ts` — **cited** (multiple times)

All source files from the brief are covered.

## Mandated Diagrams
Brief requires:
- (a) flowchart of prompt assembly layers — **present** (Diagram 1)
- (b) classDiagram of prompt fragment sources — **missing as classDiagram**; a flowchart of the CLAUDE.md loading hierarchy is provided instead

The second diagram is a flowchart, not a classDiagram as required. This is a gap in diagram type compliance.

## Minimum Counts
- Citation count: 15 (>= 6) — pass
- Diagram count: 2 (>= 2) — pass
- Snippet count: 4 (>= 4) — pass

## Top Source Files Without Snippets
Top 3 source files by centrality:
1. `src/constants/prompts.ts` — has snippet (SYSTEM_PROMPT_DYNAMIC_BOUNDARY)
2. `src/utils/claudemd.ts` — has snippets (MemoryFileInfo, MEMORY_INSTRUCTION_PROMPT)
3. `src/utils/context.ts` — has snippet (getContextWindowForModel)

All top 3 files have at least one snippet. Pass.

## Uncovered Topics
1. **Effort and plan mode in prompt assembly**: The synopsis mentions "effort, plan mode" as prompt assembly inputs, but the chapter does not discuss how effort level or plan mode modifies the system prompt. While these are covered in other chapters, the chapter should acknowledge the handoff.
2. **Tool schema injection**: The chapter mentions tools in the assembly diagram but does not discuss how tool schemas are injected into the system prompt or how they are serialized for the API call.
3. **Hooks as prompt source**: The synopsis mentions "hooks" but the chapter does not discuss how hooks contribute to the system prompt content.

## Summary
- 0 uncited brief files
- 1 diagram type mismatch (flowchart instead of classDiagram)
- 3 uncovered topics
- Citation/diagram/snippet counts all meet minimums
