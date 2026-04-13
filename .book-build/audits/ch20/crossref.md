# Crossref Audit: Chapter 20

## Required HER References

1. **§7.4 Sub-agents configuration surface** - FOUND. Referenced in the Overview (line 7), the "Where cc diverges" section (line 493), and throughout the chapter. The chapter directly engages with how cc's agent definitions implement HER's subagent configuration surface concept.

2. **§5 Pattern 2 Scoped Context Assembly** - FOUND. Referenced in the Overview (line 7), the control flow section discussing precedence (line 270), and the divergence section (line 495). The chapter connects agent discovery's layered override model to Pattern 2.

## Divergence Section Analysis

The "Where cc diverges from the published pattern" section (lines 491-501) covers:

1. HER §7.4 - cc's agent definitions go further than HER describes with 22+ configurable fields (substantive, ~80 words on this point)
2. HER Pattern 2 - cc's agent discovery follows similar layered approach but doesn't support directory-scoped agents (substantive)
3. The `omitClaudeMd` flag - cc-specific optimization not in HER (substantive)
4. The `requiredMcpServers` filtering - not described in HER (substantive)
5. The `memory` scope and snapshot mechanism - cc-specific features (substantive)

Total divergence section word count: approximately 280 words. This exceeds the 150-word minimum.

## Issues

- No missing required refs
- No wrong section references
- Divergence section is substantive (>150 words)

## Verdict: pass
