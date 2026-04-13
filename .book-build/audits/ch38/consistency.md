# Consistency Audit: Chapter 38

## Term Conflicts

1. **progressive tool expansion** vs "Progressive Disclosure"
   - The terminology registry defines "progressive tool expansion" as the canonical term
   - The chapter uses "Progressive Disclosure" in the title and throughout, and references HER Pattern 9 (Progressive Tool Expansion)
   - The title "Skills: Discovery, Frontmatter, Progressive Disclosure" uses the non-canonical phrasing
   - Conflict: the chapter conflates two distinct concepts - "progressive tool expansion" (the HER pattern) and "progressive disclosure" (the UI/information design pattern)

2. **skill** vs "slash command" ambiguity
   - The chapter correctly describes skills and their relationship to legacy commands
   - But the `commands_DEPRECATED` LoadedFrom value is described as "legacy slash commands" when the registry defines "slash command" as a separate concept
   - Minor: the distinction is mostly clear from context

## Voice Drift
- The chapter uses informal language in the Edge cases section: "A malicious `.claude/skills/pwn/SKILL.md`" and "files: { '../../../etc/passwd': 'pwned' }" - while illustrative, this breaks the cite-heavy, descriptive register
- Otherwise the chapter is well-aligned with the house style (present tense, descriptive, cite-heavy)

## Proposed New Terms
1. safe-properties auto-allow - The optimization in SkillTool.checkPermissions that auto-allows skills whose only non-trivial properties are in the SAFE_SKILL_PROPERTIES set
2. conditional skill - A skill with a paths frontmatter field that is not listed until matching file paths are encountered
3. dynamic skill discovery - The process of discovering skills from .claude/skills directories encountered during file operations

## Verdict: revise (2 term conflicts, mild voice drift)
