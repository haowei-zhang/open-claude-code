# Accuracy Audit: Chapter 26 — Memdir: Tiered Memory on Disk

## Verdict: REWRITE

### Critical Issue: Hallucinated Snippet

The snippet at chapter line 632 claims `src/utils/attachments.ts:L282` contains:
```typescript
export const memoryHeader = 'memory'
```
This is **incorrect**. Line 282 of attachments.ts actually contains `MAX_SESSION_BYTES: 60 * 1024,` (part of RELEVANT_MEMORIES_CONFIG). The real `memoryHeader` is an exported **function** at line 2327 with signature `(path: string, mtimeMs: number): string` that constructs a staleness-aware header string from path and mtimeMs. It is not a string constant. The surrounding prose claim that this tag "enables the API-level token accounting to distinguish memory-injected content from other file attachments" is also unsupported by the actual implementation.

### Snippet Drift Issues

Three snippets use heavy `// ...` omission markers, showing structural outlines rather than actual code:
1. `src/memdir/memdir.ts:L199-L266` (buildMemoryLines) — chapter line 384
2. `src/memdir/memdir.ts:L419-L507` (loadMemoryPrompt) — chapter line 441
3. `src/memdir/memdir.ts:L327-L370` (buildAssistantDailyLogPrompt) — chapter line 590

### Verified Snippets (21 verbatim)

All other snippets with source-path captions match the actual source files character-by-character:
- memoryTypes.ts: MEMORY_TYPES, MEMORY_FRONTMATTER_EXAMPLE, WHAT_NOT_TO_SAVE_SECTION, TRUSTING_RECALL_SECTION, WHEN_TO_ACCESS_SECTION
- memoryScan.ts: MemoryHeader type, formatMemoryManifest, scanMemoryFiles
- findRelevantMemories.ts: RelevantMemory type, findRelevantMemories, SELECT_MEMORIES_SYSTEM_PROMPT, hallucinated filename filter
- paths.ts: isAutoMemoryEnabled, validateMemoryPath, getAutoMemPathSetting, getAutoMemPath
- memdir.ts: truncateEntrypointContent, DIR_EXISTS_GUIDANCE
- memoryAge.ts: memoryAgeDays, memoryFreshnessText

### Uncited Sources

All 6 brief source files are cited in the chapter.

### Summary

| Metric | Value |
|--------|-------|
| Total citations | 27 |
| Verified citations | 23 |
| Snippet total | 27 |
| Snippet verbatim | 21 |
| Snippet drift | 5 |
| Snippet hallucinated | 1 |
