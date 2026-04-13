# Consistency Audit: Chapter 20

## Term Conflict Check

Scanning chapter 20 against terminology.json:

1. **subagent** (canonical) - Chapter uses "subagent" throughout (e.g., line 7, 73, 98). Also uses "sub-agent" at line 495: "cc does not support directory-scoped agents (agents that activate only when the working directory matches a glob pattern)." Wait, re-checking: the chapter uses "subagent" consistently. The term "sub-agent" does not appear. No conflict.

2. **tool** (canonical) - Used correctly throughout.

3. **skill** (canonical) - Used correctly.

4. **hook** (canonical) - Used correctly in context of `hooks` field.

5. **permission mode** (canonical) - Used correctly ("permissionMode" field references).

6. **feature gate** (canonical) - Not used in this chapter.

7. **lazySchema** (canonical) - Referenced correctly in code snippets.

8. **compaction** - Not referenced in this chapter.

9. **harness** - Used once in context of HER, correctly.

10. **memory** (canonical) - Used correctly with scope types (user, project, local).

11. **dispatch pipeline** - Not directly referenced.

12. **deferred tool** / **ToolSearch** - Not referenced in this chapter.

13. **prompt assembly** - Referenced indirectly in the getSystemPrompt discussion; no conflict.

14. **frontmatter** - This term is used extensively and consistently throughout the chapter. It is not in the terminology registry but should be proposed.

15. **agent definition** - Used consistently. Not in terminology registry; should be proposed.

16. **precedence** - Used in the context of agent discovery ordering. Consistent with the settings cascade concept from terminology.

17. **plugin** - Used consistently with the PluginAgentDefinition type.

18. **setting source** - Used as `SettingSource` type, consistent with canonical usage.

## Voice Drift Check

The chapter is written in present tense, descriptive, cite-heavy style consistent with house style. No voice drift detected.

## Proposed New Terms

1. **agent definition** - "A typed configuration object (built-in, custom, or plugin variant) that specifies an agent's tools, permissions, model, memory scope, and system prompt, discovered from multiple sources with layered precedence."

2. **frontmatter** - "A YAML header delimited by --- markers at the top of a Markdown file, parsed into typed fields that configure agents, skills, or slash commands."

3. **agent discovery** - "The process of loading agent definitions from multiple sources (built-in, plugin, user, project, flag, policy) with later sources overriding earlier ones for the same agentType."

4. **MCP requirement filtering** - "The mechanism that removes agents from the active list when their declared requiredMcpServers do not match any configured MCP servers, preventing runtime failures from missing infrastructure."

## Verdict: pass (zero term conflicts, no voice drift)
