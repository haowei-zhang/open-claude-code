# Consistency Audit: Chapter 9 — System Prompts and Prompt Assembly

## Term Conflicts

1. **"system prompt"** vs terminology registry: The registry does not have a "system prompt" term. The chapter uses "system prompt" consistently throughout. No conflict.

2. **"subagent"** — used correctly per registry definition ("An agent spawned by the parent agent via AgentTool"). Chapter uses it once in the simple mode variant discussion: "subagents spawned via AgentTool". Consistent.

3. **"compaction"** — not used in this chapter. No conflict.

4. **"observation masking"** — not used in this chapter. No conflict.

5. **"deferred tool"** — not used in this chapter. No conflict.

6. **"feature gate"** — chapter uses "feature flags" on several occasions. The registry defines "feature gate" as "A compile-time boundary checked via feature() from bun:bundle". The chapter's usage of "feature flags" refers to the same concept but uses a different term. This is a minor conflict — "feature gate" is the canonical term per the registry.

7. **"simple mode"** — chapter refers to `CLAUDE_CODE_SIMPLE` environment variable. Not in terminology registry. No conflict.

## Voice Drift
- The chapter is written in present tense, descriptive, cite-heavy style consistent with house style.
- No voice drift detected.

## Proposed New Terms
1. **prompt assembly** — The process of composing the system prompt from multiple sections in a specific layer order, including static cacheable sections and dynamic per-session sections separated by a boundary marker.
2. **cache boundary** — A control token (SYSTEM_PROMPT_DYNAMIC_BOUNDARY) that splits the system prompt array into static (globally cacheable) and dynamic (per-session) segments for API-level prompt caching.
3. **memory loading hierarchy** — The priority-ordered cascade of CLAUDE.md file loading: managed, user, project, local, auto-mem, team-mem, where later-loaded files take precedence.
4. **conditional rule scoping** — The mechanism by which .claude/rules/*.md files with frontmatter paths: globs are filtered at query time to inject only instructions relevant to the current file being operated on.

## Summary
- 1 minor term conflict (feature flags vs feature gate)
- No voice drift
- 4 proposed new terms
