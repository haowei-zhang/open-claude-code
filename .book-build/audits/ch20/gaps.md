# Gaps Audit: Chapter 20

## 1. Overview vs Synopsis Match

**Synopsis from brief**: "How cc discovers and loads agents from `~/.claude/agents`, plugin agents, built-ins. Markdown + YAML frontmatter contract."

**Chapter Overview**: Covers discovery and loading from multiple sources (built-in, user, project, plugin, managed), YAML frontmatter parsing, agent definition types, and the discovery pipeline. Matches the synopsis well.

## 2. Source File Citation Check

Brief source files:
1. `src/tools/AgentTool/loadAgentsDir.ts` - CITED extensively (13 snippet references)
2. `src/utils/plugins/loadPluginAgents.ts` - CITED (2 snippet references)
3. `src/utils/frontmatterParser.ts` - CITED (3 snippet references)

All 3 source files cited at least once. No uncited brief files.

## 3. Mandated Diagrams

Brief requires:
1. (a) flowchart of agent discovery across directories - PROVIDED (line 272)
2. (b) classDiagram of AgentDefinition shape - PROVIDED (line 159)

Both mandated diagrams present.

## 4. Minimum Counts

- Citation count: 13 (minimum 6) - PASS
- Diagram count: 2 (minimum 2) - PASS
- Snippet count: 13 (minimum 4) - PASS

## 5. Top 3 Source Files Snippet Check

Top 3 source files:
1. `src/tools/AgentTool/loadAgentsDir.ts` - Has multiple snippets (8+)
2. `src/utils/frontmatterParser.ts` - Has snippets (3)
3. `src/utils/plugins/loadPluginAgents.ts` - Has snippets (2)

All top 3 files have at least one snippet.

## 6. Missing Topics

Topics a reasonable reader would expect based on the brief:
- Built-in agent definitions: The chapter references `getBuiltInAgents()` in code but does not detail what built-in agents exist or how they are defined. This is a minor gap since the brief mentions "built-ins" as a topic.
- The `builtInAgents.ts` file is not in the source_files list, so this is acceptable.
- Plugin security model: The chapter mentions in the code that `permissionMode`, `hooks`, `mcpServers` are intentionally NOT parsed for plugin agents (lines 153-168 in loadPluginAgents.ts), but the chapter's prose does not discuss this important security decision. This is a gap for a reasonable reader.
- `strictPluginOnlyCustomization` mode: The chapter's edge cases section mentions this (line 485) but only briefly.

## Verdict: pass (all source files cited, all diagrams present, counts sufficient, minor topic gaps but none severe enough to fail)
