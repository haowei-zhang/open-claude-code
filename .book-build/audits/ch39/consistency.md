# Consistency Audit: Chapter 39

## Terminology Consistency Check

### Terms from terminology.json used in this chapter:

1. **slash command** - Used correctly throughout. Canonical definition: "A user-invocable command parsed from the REPL input line, registered in commands.ts as either prompt or callback type." Chapter usage is consistent.

2. **skill** - Used correctly. Canonical definition: "A reusable, frontmatter-defined capability discovered from ~/.claude/skills or bundled, invoked inline or in a forked context." Chapter discusses skills as command sources, consistent with the definition.

3. **frontmatter** - Used correctly. Canonical definition: "A YAML header delimited by --- markers at the top of a Markdown file, parsed into typed fields that configure agents, skills, or slash commands." Chapter uses it in context of skill loading.

4. **feature gate** - Used correctly. Canonical definition: "A compile-time boundary checked via feature() from bun:bundle that determines which code exists in the external build." Chapter discusses feature-gated commands.

5. **deferred tool** - Referenced implicitly. Not used with alternate naming.

6. **ToolSearch** - Referenced in context of progressive disclosure. Not used as alternate name.

7. **progressive tool expansion** - Referenced correctly in divergence section as "HER Pattern 9 (Progressive Tool Expansion)".

8. **prompt assembly** - Not directly used in this chapter.

9. **hook** - Referenced in PromptCommand fields. Used consistently with the canonical definition.

### Term Conflicts

- None detected. All registered terms are used with their canonical definitions.

### Voice Drift

- The chapter maintains the house style of "present tense, descriptive, cite-heavy" throughout.
- No voice drift detected.

### Proposed New Terms

1. **command registry** - The memoized collection of all Command objects assembled from built-in, skill-directory, plugin, bundled, and MCP sources, filtered by availability and feature gates.
2. **bridge safety predicate** - The isBridgeSafeCommand function that classifies commands into three tiers (prompt=safe, local-jsx=blocked, local=allowlist) for remote execution authorization.
3. **command shadowing** - The precedence mechanism where commands loaded earlier in the array take priority over later commands with the same name during findCommand lookup.
