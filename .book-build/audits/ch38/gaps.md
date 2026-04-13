# Gaps Audit: Chapter 38

## Brief Compliance
- Synopsis match: The chapter covers skill directories, bundled skills, frontmatter fields, user-invocable vs triggered, inline vs fork context - matches the brief synopsis
- All 3 source files cited: src/skills/loadSkillsDir.ts, src/skills/bundledSkills.ts, src/tools/SkillTool/SkillTool.ts

## Uncited Brief Files
None - all 3 source files are cited

## Missing Diagrams
The brief requires 3 diagrams:
- (a) flowchart of skill discovery - PRESENT (SkillTool dispatch flowchart, though focused on invocation rather than discovery)
- (b) stateDiagram-v2 of skill invocation modes (inline/fork) - PRESENT
- (c) classDiagram of skill frontmatter - MISSING (replaced by code snippets showing the type definition, but no mermaid classDiagram)

## Uncovered Topics
1. The chapter does not discuss the SkillTool's output schema or how results are returned differently for inline vs forked execution
2. The chapter does not cover the SkillTool's description/prompt text that the model sees in the tool listing

## Minimum Counts
- Citations: 13 (>= 6 requirement met)
- Diagrams: 3 (>= 2 requirement met)
- Snippets: 13 (>= 4 requirement met)

## Top Files Without Snippets
None - all 3 source files have snippets

## Verdict: revise (1 missing mandated diagram, 2 uncovered topics)
