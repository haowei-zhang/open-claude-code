# Accuracy Audit: Chapter 38

## Summary
Chapter 38 cites 13 source locations across 3 files (src/skills/loadSkillsDir.ts, src/skills/bundledSkills.ts, src/tools/SkillTool/SkillTool.ts). All cited files exist and all line numbers are valid or near-valid. No hallucinated snippets were found.

## Issues Found

### 1. Snippet drift: loadSkillsDir.ts:L679-L714 (chapter line ~96)
The chapter shows the Promise.all parallel loading block but omits the conditional guards that wrap each loader call (isEnvTruthy, isSettingSourceEnabled, skillsLocked). The simplified version accurately represents the logical structure but is not a verbatim excerpt.

### 2. Snippet drift: SkillTool.ts:L122-L289 (chapter line ~263)
The chapter shows executeForkedSkill but replaces approximately 50 lines of telemetry/event-logging code with a single comment `// Report progress for tool uses`. The structural logic matches but significant code is omitted.

### 3. Snippet drift: bundledSkills.ts:L186-L193 (chapter line ~350)
The chapter cites L186-L193 but the SAFE_WRITE_FLAGS const declaration starts at L180. The actual function safeWriteFile matches, but the line range for the constant is slightly off.

## Verdict: revise (3 snippet_drift issues, no hallucinated snippets, snippet count >= 4)
