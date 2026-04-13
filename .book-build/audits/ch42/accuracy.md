# Accuracy Audit: Chapter 42 — The Ink Renderer and Terminal Engine

## Citation Verification

### Verified Citations
- `src/ink/ink.tsx` — exists, all line-number references checked
- `src/ink/screen.ts` — exists, all line-number references checked
- `src/ink/output.ts` — exists, all line-number references checked
- `src/ink/Ansi.tsx` — exists, line-number references checked

### Snippet Verification

1. **`src/ink/ink.tsx:L76-L123`** (Ink class fields): DRIFT. The chapter shows `cursorDeclaration` and `displayCursor` fields that do not exist in the actual source at those lines. The actual source has `exitPromise`, `restoreConsole`, `restoreStderr`, `unsubscribeTTYHandlers`, `terminalColumns`, `terminalRows`, `currentNode`, `drainTimer`, `lastYogaCounters`, `altScreenParkPatch` at those lines instead. The chapter snippet is a simplified reconstruction, not a verbatim extract.

2. **`src/ink/screen.ts:L21-L53`** (CharPool): VERBATIM. Matches the actual source exactly.

3. **`src/ink/screen.ts:L112-L146`** (StylePool): NEAR-VERBATIM. The `transition()` method body extends beyond L146 but the shown lines match.

4. **`src/ink/output.ts:L62-L97`** (Operation types): NEAR-VERBATIM. The shown types match, though the chapter omits some JSDoc comments.

5. **`src/ink/output.ts:L170-L199`** (Output class): DRIFT. The chapter shows a simplified `reset()` method that omits actual code lines and adds summary comments.

6. **`src/ink/Ansi.tsx:L32-L109`** (Ansi component): DRIFT. The actual source uses React compiler runtime (`_c(12)`, `$` memo slots), producing code that is structurally very different from the clean version shown. The logic is equivalent but the representation is not verbatim.

7. **`src/ink/ink.tsx:L44-L53`** (ALT_SCREEN_ANCHOR_CURSOR): NEAR-VERBATIM. Close match.

8. **`src/ink/screen.ts:L186-L200`** (withCurrentMatch): DRIFT. The chapter shows a simplified version. The actual code includes underline code additions and more complex filter logic.

9. **`src/ink/screen.ts:L1404-L1418`** (MCP cleanup): HALLUCINATED. The chapter attributes MCP client cleanup code to `src/ink/screen.ts:L1404-L1418`, but the actual code at those lines is `blitRegion` logic for screen resizing. The MCP cleanup code belongs in `src/services/mcp/client.ts`, and the chapter even acknowledges this parenthetically. This snippet should not be in this chapter.

10. **`src/ink/ink.tsx:L239-L258`** (onComputeLayout): NEAR-VERBATIM. Close match.

11. **`src/ink/ink.tsx:L210-L216`** (scheduleRender): NEAR-VERBATIM. Close match.

12. **`src/ink/ink.tsx:L471-L494`** (captureScrolledRows): DRIFT. Simplified version of the actual code.

13. **`src/ink/screen.ts:L244-L258`** (withSelectionBg): NEAR-VERBATIM. Close match.

14. **`src/ink/screen.ts:L332-L348`** (packWord1): VERBATIM. Matches exactly.

15. **`src/ink/screen.ts:L353-L354`** (EMPTY_CELL_VALUE): VERBATIM.

16. **`src/ink/screen.ts:L389-L392`** (noSelect): VERBATIM.

17. **`src/ink/output.ts:L323-L324`** (clip intersection): VERBATIM.

18. **`src/ink/output.ts:L219-L221`** (shift): VERBATIM.

## Summary

- 1 hallucinated snippet (MCP cleanup attributed to screen.ts)
- 5 drift snippets (simplified/reconstructed code that doesn't match verbatim)
- 12 verbatim/near-verbatim snippets
- Total snippets: 18 (above minimum of 4)
- All 4 source files are cited

## Unsupported Claims

- The "In-process server cleanup" subsection (snippet 9) discusses MCP server cleanup which is off-topic for a chapter about the Ink renderer and the snippet is attributed to the wrong file.

## Uncited Sources

All four listed source files are cited.
