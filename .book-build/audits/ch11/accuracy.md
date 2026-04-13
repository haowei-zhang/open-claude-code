# Accuracy Audit: Chapter 11 — Anatomy of a Tool

## Citation Verification

17 citations found. 12 verified exactly, 5 have issues.

### Issues

1. **Line number drift** (3 instances): `src/Tool.ts:L367-L369` should be L371 (aliases), `src/Tool.ts:L374-L378` should be L378 (searchHint), `src/Tool.ts:L439-L449` should be L442-L449 (shouldDefer/alwaysLoad). These are off by 2-4 lines.

2. **Snippet trimming** (2 instances): The snippet citing `src/Tool.ts:L379-L404` omits internal JSDoc comments and extra fields (inputJSONSchema, outputSchema, inputsEquivalent) present in the actual source range. The snippet citing `src/services/tools/toolExecution.ts:L615-L680` shows only 6 lines but references a 65-line range.

### Snippet Verification

- 16 snippets total (above minimum of 4)
- 11 verbatim, 5 drift, 0 hallucinated
- No uncited source files (both brief files are cited)

### Verdict: revise

Line number drift and non-verbatim snippets need correction before pass.
