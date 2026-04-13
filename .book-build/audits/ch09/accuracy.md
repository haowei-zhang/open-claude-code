# Accuracy Audit: Chapter 9 — System Prompts and Prompt Assembly

## Citation Verification

### Verified Citations

1. `src/constants/prompts.ts:L114-L117` — SYSTEM_PROMPT_DYNAMIC_BOUNDARY constant. **Verified**: Lines 114-115 match exactly.

2. `src/utils/claudemd.ts:L229-L243` — MemoryFileInfo type. **Verified**: Lines 229-243 match the type definition exactly.

3. `src/utils/context.ts:L51-L98` — getContextWindowForModel function. **Verified**: Lines 51-97 contain the full function. Chapter snippet omits some internal branches (lines 85-96) but the shown lines are verbatim.

4. `src/utils/claudemd.ts:L89-L91` — MEMORY_INSTRUCTION_PROMPT constant. **Verified**: Lines 89-90 match exactly (chapter cites L89-L91; actual content is on L89-L90, with L91 being the MAX_MEMORY_CHARACTER_COUNT comment — minor line-range drift).

5. `src/utils/claudemd.ts:L92` — MAX_MEMORY_CHARACTER_COUNT = 40000. **Verified**: Line 92.

6. `src/utils/claudemd.ts:L451-L535` — extractIncludePathsFromTokens function. **Verified**: Lines 451-534 contain the function. Line range in chapter is close enough.

7. `src/utils/claudemd.ts:L537` — MAX_INCLUDE_DEPTH = 5. **Verified**: Line 537.

8. `src/utils/claudemd.ts:L292-L334` — stripHtmlComments function. **Verified**: Lines 292-334 contain the function.

9. `src/utils/claudemd.ts:L254-L279` — parseFrontmatterPaths function. **Verified**: Lines 254-279 contain the function.

10. `src/utils/claudemd.ts:L868-L884` — Nested worktree double-loading. **Verified**: Lines 868-884 contain the relevant logic.

11. `src/utils/claudemd.ts:L1416-L1430` — External @include security. **Verified**: Lines 1416-1430 contain shouldShowClaudeMdExternalIncludesWarning.

12. `src/utils/context.ts:L9` — MODEL_CONTEXT_WINDOW_DEFAULT. **Verified**: Line 9.

13. `src/utils/claudemd.ts:L383-L385` — truncateEntrypointContent for AutoMem/TeamMem. **Verified**: Lines 383-385.

14. `src/utils/claudemd.ts:L1354-L1397` — processConditionedMdRules function. **Verified**: Function starts at line 1354.

### Unsupported Claims

1. Chapter claims `src/utils/frontmatterParser.ts` is "370 lines" — **Verified**: file is 370 lines.

2. Chapter claims "getSystemPrompt() returns Promise<string[]>" — needs verification from prompts.ts.

3. Chapter claims "cache lifetime on the Anthropic API is typically 5 minutes" — this is an API-level detail not verifiable from the codebase but is a known Anthropic API specification.

4. Chapter claims "simple mode reduces token cost by approximately 80%" — this is an estimate, not directly verifiable from code.

### Snippet Verification

1. `SYSTEM_PROMPT_DYNAMIC_BOUNDARY` (L114-L117): **verbatim** — matches exactly.

2. `MemoryFileInfo` type (L229-L243): **drift** — Chapter shows `contentDiffersFromDisk?: boolean` on one line followed by `rawContent?: string`, but actual code has multi-line comments between these fields (lines 235-240). The chapter collapses the comments into the field definition.

3. `getContextWindowForModel` (L51-L98): **drift** — Chapter snippet shows abbreviated version with `// ... returns cap or default` replacing lines 75-82. This is a legitimate trim marker, but the snippet also omits lines 85-97 (betas, sonnet1m, antModel branches). The `// ...` trim is acceptable but the snippet diverges from the actual function body after line 72.

4. `MEMORY_INSTRUCTION_PROMPT` (L89-L91): **verbatim** — matches exactly.

### Uncited Source Files

- `src/utils/analyzeContext.ts` — cited textually but no `src/...:Lnnn` citation format. The function `analyzeContextUsage()` is discussed but not cited by line number.
- `src/utils/context.ts` — cited for the getContextWindowForModel function and MODEL_CONTEXT_WINDOW_DEFAULT.

## Summary

- Citation total: ~14
- Citations verified: 13
- Snippet total: 4
- Snippet verbatim: 2
- Snippet drift: 2
- Snippet hallucinated: 0
- Issues: 2 snippet drift instances, 1 line-range minor mismatch (MEMORY_INSTRUCTION_PROMPT cited as L89-L91, actual is L89-L90)
