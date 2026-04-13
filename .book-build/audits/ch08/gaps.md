# Gaps Audit: Chapter 8 — Talking to Anthropic: Streaming

## Brief Synopsis Match
The brief synopsis says: "Anthropic SDK wrapper. Streaming, fallback model, token metering, retries, prompt cache break detection, rate limits, first-token date, usage accumulation."

The chapter covers:
- Streaming: YES (streaming request lifecycle section)
- Fallback model: YES (fallback model selection section, state diagram)
- Token metering: YES (usage accumulation and cost tracking section)
- Retries: YES (withRetry wrapper section)
- Prompt cache break detection: YES (dedicated section)
- Rate limits: PARTIALLY — 429 errors are mentioned in the retry section and quota status extraction, but there is no dedicated rate limit discussion
- First-token date: MISSING — not mentioned anywhere in the chapter
- Usage accumulation: YES (dedicated section)

## Source File Citation

Brief source files:
1. `src/services/api/claude.ts` — CITED (primary source throughout)
2. `src/services/api/usage.ts` — NOT CITED (usage tracking is referenced via cost-tracker.ts instead)
3. `src/services/api/errors.ts` — NOT DIRECTLY CITED (error handling is discussed but the file is not named)
4. `src/services/api/logging.ts` — CITED (NonNullableUsage type, logAPISuccessAndDuration function)

Uncited brief files: 2 (usage.ts, errors.ts)

## Required Diagrams

Brief requires:
1. (a) sequenceDiagram of a streaming request with retry — PRESENT
2. (b) stateDiagram-v2 of request lifecycle including fallback — PRESENT
3. (c) flowchart of usage accounting and cost hooks — MISSING

## Minimum Counts
- Citations: 18 (≥ 6) — PASS
- Diagrams: 2 (≥ 2) — PASS (but 1 required diagram missing)
- Snippets: 5 (≥ 4) — PASS

## Top 3 Source Files Without Snippets
Top 3 source files by centrality:
1. `src/services/api/claude.ts` — HAS snippets (5 snippets reference this file)
2. `src/services/api/logging.ts` — HAS snippet (1 snippet)
3. `src/services/api/usage.ts` — NO snippet

## Uncovered Topics
1. First-token latency (first-token date) — mentioned in brief but not covered
2. Rate limit handling as a distinct topic — partially covered via 429 retry but not as a dedicated section
3. `src/services/api/usage.ts` — the usage tracking module is not discussed despite being in the brief
