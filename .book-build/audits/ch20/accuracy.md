# Accuracy Audit: Chapter 20

## Citation Verification

### Snippet 1: BaseAgentDefinition (L106-L165)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L106-L165
- Actual content at L106-L165 matches closely. The chapter omits comments on some fields (e.g., `skills?: string[] // Skill names to preload...` is shown without the comment, `mcpServers` comment is shown, `hooks` comment shown, `maxTurns` comment shown). The `omitClaudeMd` field in the actual source has a multi-line JSDoc comment; the chapter shows it without the comment.
- Verdict: **drift** - comments trimmed from type fields

### Snippet 2: Type guards (L168-L184)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L168-L184
- Actual: L168-L184 matches exactly.
- Verdict: **verbatim**

### Snippet 3: AgentMcpServerSpec (L58-L61)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L58-L61
- Actual: L58-L60 is the type; L61 is blank. The chapter shows 4 lines (L58-L61) which matches.
- Verdict: **verbatim**

### Snippet 4: AgentMcpServerSpecSchema (L63-L68)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L63-L68
- Actual: L62-L68 includes the comment "// Zod schema for agent MCP server specs" on L62. The chapter starts at L63 (skipping the comment). Content matches.
- Verdict: **verbatim** (comment omission is acceptable per trim markers rule)

### Snippet 5: AgentJsonSchema (L73-L99)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L73-L99
- Actual: The chapter shows the schema but with minor formatting differences. The `model` field in the actual source spans L79-L84 as a chained call. The chapter wraps it differently (`.transform(m => (m.toLowerCase() === 'inherit' ? 'inherit' : m))` on one line). In the actual source, the `model` transform line is `m.toLowerCase() === 'inherit' ? 'inherit' : m` which matches. Content is structurally identical but the chapter shows it in slightly different line formatting.
- Verdict: **drift** - minor whitespace/formatting differences

### Snippet 6: FrontmatterData (L10-L59)
- File: src/utils/frontmatterParser.ts
- Claim: L10-L59
- Actual: The actual source includes comments on each field. The chapter omits all the inline comments. The field names and types match exactly.
- Verdict: **drift** - comments trimmed

### Snippet 7: getActiveAgentsFromList (L193-L221)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L193-L221
- **ISSUE**: The chapter shows `flagAgents` at position 5 (after projectAgents) and `managedAgents` at position 6, matching the actual source code order. However, the chapter shows the `agentGroups` array in the order: `[builtInAgents, pluginAgents, userAgents, projectAgents, flagAgents, managedAgents]` which matches the actual source. BUT the chapter snippet at L263 shows `})` on its own line instead of `})` at the end of the for-of body. This is a minor formatting difference.
- **CRITICAL ISSUE**: In the actual source at L214-217, the loop body is `agentMap.set(agent.agentType, agent)` followed by `})` on the next line, but looking at the actual code it's actually:
```
for (const agents of agentGroups) {
    for (const agent of agents) {
      agentMap.set(agent.agentType, agent)
    }
  }
```
The chapter shows `})` on line 263 which is incorrect formatting - the actual source has proper closing braces.
- Verdict: **drift** - minor formatting

### Snippet 8: parseFrontmatter (L130-L175)
- File: src/utils/frontmatterParser.ts
- Claim: L130-L175
- Actual: Content matches closely. The actual source includes comments like "// No frontmatter found" and "// YAML parsing failed - try again after quoting problematic values" which are trimmed in the chapter. The log message format differs slightly: actual source includes `${location}` variable with `sourcePath` which the chapter abbreviates as `...`.
- Verdict: **drift** - comments trimmed, log message abbreviated

### Snippet 9: quoteProblematicValues (L67-L121)
- File: src/utils/frontmatterParser.ts
- Claim: L67-L121
- Actual: The actual source starts the function at L85, not L67. L67 is the comment block for YAML_SPECIAL_CHARS. The YAML_SPECIAL_CHARS regex definition is at L79. The chapter shows L67-L121 which would include the comment block starting at L67. Content matches structurally.
- Verdict: **drift** - line range includes comments that are partially shown

### Snippet 10: loadAgentsFromDirectory (L37-L63)
- File: src/utils/plugins/loadPluginAgents.ts
- Claim: L37-L63
- Actual: Content matches exactly.
- Verdict: **verbatim**

### Snippet 11: nameParts namespace (L85-L90)
- File: src/utils/plugins/loadPluginAgents.ts
- Claim: L85-L90
- Actual: Content matches exactly.
- Verdict: **verbatim**

### Snippet 12: hasRequiredMcpServers / filterAgentsByMcpRequirements (L229-L255)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L229-L255
- Actual: Content matches closely. The actual source includes JSDoc comments which the chapter trims. The actual `hasRequiredMcpServers` includes a comment `// Each required pattern must match at least one available server (case-insensitive)` which is omitted in the chapter.
- Verdict: **drift** - comments trimmed

### Snippet 13: initializeAgentMemorySnapshots (L262-L294)
- File: src/tools/AgentTool/loadAgentsDir.ts
- Claim: L262-L294
- Actual: Content matches closely. The actual source includes `logForDebugging` calls inside the `initialize` and `prompt-update` cases which the chapter omits.
- Verdict: **drift** - log statements trimmed

## Summary
- 13 snippets total (>= 4 minimum)
- 4 verbatim, 9 drift, 0 hallucinated
- No hallucinated snippets
- No bad citations or unsupported claims detected
- All source files are cited

## Verdict: revise (due to snippet drift)
