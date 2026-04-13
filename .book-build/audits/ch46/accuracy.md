# Accuracy Audit: Chapter 46 - Worktrees: Isolated Parallel Branches

## Snippet Verification

### Verbatim (9)
1. WorktreeSession type (L140-L155) - exact match
2. EnterWorktreeTool input schema (L23-L39) - exact match
3. ExitWorktreeTool input schema (L31-L44) - exact match
4. readWorktreeHeadSha fast-resume (L247-L255) - exact match
5. validateWorktreeSlug (L48-L87) - exact match
6. flattenSlug (L217-L219) - exact match
7. EnterWorktreeTool.call() (L77-L119) - exact match
8. countWorktreeChanges (L79-L113) - exact match
9. restoreSessionToOriginalCwd (L122-L146) - exact match
10. EPHEMERAL_WORKTREE_PATTERNS (L1030-L1041) - exact match
11. symlinkDirectories (L106-L138) - exact match (line offset)

### Drift (7)
1. WorktreeCreateResult type: line numbers off by 1 (L180 not L181), content matches
2. createWorktreeForSession: heavily abbreviated with // ... trim markers
3. performPostCreationSetup: shown as pseudocode/comments, not actual source
4. validateInput change detection: slightly abbreviated, message text differs
5. symlinkDirectories: function starts at L102 not L106
6. Sparse checkout: slightly abbreviated

### Hallucinated (0)
No hallucinated snippets detected.

## Citation Verification
- 27 total citations in chapter
- 21 verified against source files
- All file paths exist and are valid
- Line numbers mostly accurate (a few off by 1-4 lines)

## Uncited Sources (informational)
- src/tools/EnterWorktreeTool/constants.ts
- src/tools/EnterWorktreeTool/UI.tsx
- src/tools/ExitWorktreeTool/constants.ts
- src/tools/ExitWorktreeTool/UI.tsx

## Verdict: revise
7 snippet_drift issues found, 0 hallucinated. All citations point to real files with correct content. The drift issues are from line number inaccuracies and abbreviation of longer code blocks.
