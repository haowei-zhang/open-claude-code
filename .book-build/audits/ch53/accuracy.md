# Accuracy Audit: Chapter 53

## Summary

Chapter 53 is a meta-chapter mapping 12 HER patterns to cc subsystems. It cites 37 source file references and contains 14 code snippets. No source files are listed in the manifest brief (meta-chapter), so there are no uncited brief sources.

## Snippet Verification

### Verbatim (12)
1. `memoryTypes.ts:L14-L19` - MEMORY_TYPES array and MemoryType type: exact match
2. `memdir.ts:L34-L38` - ENTRYPOINT_NAME, caps, AUTO_MEM_DISPLAY_NAME: exact match
3. `Tool.ts:L15-L21` - ToolInputJSONSchema type: exact match
4. `claudemd.ts:L1-L9` - Loading order comment (first 9 lines): exact match
5. `findRelevantMemories.ts:L39-L45` - Function signature: exact match
6. `autoDream.ts:L122-L141` - initAutoDream gate evaluation: exact match
7. `microCompact.ts:L41-L50` - COMPACTABLE_TOOLS set: exact match
8. `forkSubagent.ts:L32-L39` - isForkSubagentEnabled function: exact match
9. `ToolSearchTool.ts:L21-L34` - inputSchema: exact match
10. `bashSecurity.ts:L16-L41` - COMMAND_SUBSTITUTION_PATTERNS: exact match
11. `schemas/hooks.ts:L19-L27` - IfConditionSchema: exact match
12. `schemas/hooks.ts:L31-L65` (content that falls within the range): partial verbatim

### Drift (2)
1. `schemas/hooks.ts:L31-L65` (L76): Caption line range is inaccurate — snippet shows content from PromptHookSchema, HttpHookSchema, and AgentHookSchema that extends beyond L65. Additionally, BashCommandHookSchema omits `.describe()` on timeout and omits shell/statusMessage fields.
2. `AgentTool.tsx:L82-L88` (L280): `run_in_background` field omits `.describe('Set to true...')` from the actual source.

### Hallucinated (0)
No hallucinated snippets detected.

## Citation Verification

All 37 inline source-file citations reference real files and plausible line numbers. Key verifications:
- `memoryTypes.ts:L28-L31` - parseMemoryType confirmed at L28
- `memdir.ts:L57-L80` - truncateEntrypointContent confirmed at L57
- `claudemd.ts:L47` - getAdditionalDirectoriesForClaudeMd import confirmed
- `autoDream.ts:L143-L151` - scan throttle confirmed
- `bashSecurity.ts:L44-L60` - ZSH_DANGEROUS_COMMANDS confirmed
- `bashClassifier.ts:L6-L10` - ClassifierResult type confirmed at L5-L10
- `bashClassifier.ts:L24-L26` - isClassifierPermissionsEnabled confirmed
- `bashClassifier.ts:L40-L53` - classifyBashCommand stub confirmed
- `consolidationLock.ts:L19` - HOLDER_STALE_MS confirmed
- `planModeV2.ts:L5-L29` - getPlanModeV2AgentCount confirmed
- `planModeV2.ts:L49-L62` - isPlanModeInterviewPhaseEnabled confirmed at ~L50
- `planModeV2.ts:L64-L79` - PewterLedgerVariant confirmed

## Verdict: revise

Two snippet drift issues. No hallucinated snippets, no bad citations, snippet count meets minimum.
