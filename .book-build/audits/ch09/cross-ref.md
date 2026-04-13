# Cross-Reference Audit: Chapter 9 — System Prompts and Prompt Assembly

## Required HER References

1. **§7.1 Persistent Instruction File** — **FOUND**: Chapter extensively discusses CLAUDE.md loading hierarchy, MEMORY_INSTRUCTION_PROMPT, and the persistent instruction file system in the "CLAUDE.md loading hierarchy" section.

2. **§5 Pattern 2 Scoped Context Assembly** — **FOUND**: Chapter's "Where cc diverges from the published pattern" section explicitly references "HER Pattern 2 (Scoped Context Assembly)" and discusses multi-level instruction loading.

3. **§7.1 ETH Zurich AGENTS.md finding** — **FOUND**: Chapter has a dedicated "ETH Zurich finding: more instructions can hurt performance" section in Edge Cases, citing arXiv:2602.11988 and the 20%+ inference cost increase.

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive. It contains 4 numbered divergences with specific details:

1. Cache-aware section ordering (not in HER)
2. Conditional rule scoping with .claude/rules/*.md paths globs
3. Memory entrypoint truncation (not in HER)
4. ETH Zurich counter-evidence

Word count of divergence section: approximately 230 words. This exceeds the 150-word minimum.

## Issues

- All 3 required HER references are present and substantive.
- Divergence section is substantive and specific.
- No wrong section references found.

## Verdict

All required refs present. Divergence section ≥ 150 words. No bad refs.
