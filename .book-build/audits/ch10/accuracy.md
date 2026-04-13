# Accuracy Audit: Chapter 10 — Token Budgets, Effort, and Fast Mode

## Citation Verification

### Verified Citations

| Citation | File Exists | Line Valid | Supports Claim |
|----------|-------------|------------|----------------|
| src/query/tokenBudget.ts:L6-L11 | Yes | Yes | BudgetTracker type matches exactly |
| src/query/tokenBudget.ts:L23-L31 | Yes | Off by 1 (actual L22-L29) | ContinueDecision content matches but line numbers drift |
| src/query/tokenBudget.ts:L33-L43 | Yes | Off by 2 (actual L31-L41) | StopDecision content matches but line numbers drift |
| src/state/AppStateStore.ts:L423-L427 | Yes | Yes | fastMode/effortValue fields match |
| src/query/tokenBudget.ts:L3-L4 | Yes | Yes | COMPLETION_THRESHOLD and DIMINISHING_THRESHOLD match exactly |
| src/services/tokenEstimation.ts:L215-L224 | Yes | Yes | bytesPerTokenForFileType matches exactly |
| src/services/tokenEstimation.ts:L401-L412 | Yes | Off by 1 (actual L400-L412) | Image/document block content matches but line number drifts |

### Functional Citations (no line numbers, verified against source)

- `createBudgetTracker()` — exists at L13, correctly described
- `checkTokenBudget()` — exists at L45, logic correctly described
- `countTokensWithAPI()` — exists at L124, correctly described
- `countTokensViaHaikuFallback()` — exists at L251, correctly described
- `roughTokenCountEstimation()` — exists at L203, correctly described
- `stripToolSearchFieldsFromMessages()` — exists at L66, correctly described
- `countTokensWithBedrock()` — exists at L437, correctly described
- `hasThinkingBlocks()` — exists at L38, correctly described
- `getBudgetContinuationMessage()` — imported from utils/tokenBudget.js, correct

### Factual Claims Verified

1. COMPLETION_THRESHOLD = 0.9 — CONFIRMED (L3)
2. DIMINISHING_THRESHOLD = 500 — CONFIRMED (L4)
3. Subagent budget isolation (agentId → immediate stop) — CONFIRMED (L51)
4. TOKEN_COUNT_THINKING_BUDGET = 1024 — CONFIRMED (L32)
5. TOKEN_COUNT_MAX_TOKENS = 2048 — CONFIRMED (L33)
6. Image/document estimate = 2000 tokens — CONFIRMED (L411)
7. Bedrock doesn't support countTokens endpoint — CONFIRMED (L150-158)
8. bytes-per-token default 4, JSON 2 — CONFIRMED (L203-224)
9. Placeholder `[tool references]` for stripped content — CONFIRMED (L100-103)

### Uncited Source Files

- None; all three source files (tokenBudget.ts, tokenEstimation.ts, AppStateStore.ts) are cited.

## Snippet Verification

| # | Caption | Range Claimed | Range Actual | Verdict |
|---|---------|---------------|--------------|---------|
| 1 | BudgetTracker | L6-L11 | L6-L11 | verbatim |
| 2 | ContinueDecision | L23-L31 | L22-L29 | drift (off by 1) |
| 3 | StopDecision | L33-L43 | L31-L41 | drift (off by 2) |
| 4 | AppState fastMode/effortValue | L423-L427 | L423-L427 | verbatim |
| 5 | Thresholds | L3-L4 | L3-L4 | verbatim |
| 6 | bytesPerTokenForFileType | L215-L224 | L215-L224 | verbatim |
| 7 | Image/document estimate | L401-L412 | L400-L412 | drift (off by 1) |

**Total snippets: 7** (minimum 4 required)
- Verbatim: 4
- Drift: 3 (line number mismatches, content correct)
- Hallucinated: 0

## Issues

1. ContinueDecision snippet line range L23-L31 should be L22-L29 (drift by 1)
2. StopDecision snippet line range L33-L43 should be L31-L41 (drift by 2)
3. Image/document snippet line range L401-L412 should be L400-L412 (drift by 1)

These are minor line-number drifts — the code content itself matches.
