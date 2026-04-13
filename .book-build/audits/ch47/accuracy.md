# Accuracy Audit Report — Chapter 47

## Summary

Verdict: **revise**

15 snippets total. 10 verbatim, 4 drift, 1 hallucinated.

## Critical Issue

- **CronDeleteTool.ts:L42-L56 (line 408)**: The chapter cites this range and shows a `call()` method returning `{ data: { message: ... } }`. The actual code at L42-56 contains the `outputSchema` getter, `isEnabled()`, `description()`, `prompt()`, and `getPath()`. The actual `call()` method is at L82-85 and returns `{ data: { id } }`, not `{ data: { message: ... } }`. This is a hallucinated snippet — the code shown does not exist at the cited location.

## Drift Issues

- **CronCreateTool.ts:L82-L116 (line 258)**: Minor whitespace and comment formatting drift. The teammate durable-cron comment in the source differs slightly from the chapter representation.
- **cron.ts:L119-L181 (line 126)**: The chapter snippet omits the domSet/dowSet constructions and smart-jump logic (lines 125-131, 148-165), replacing them with `// ...` trim markers. Acceptable trim but constitutes drift from the actual line range.
- **cronTasks.ts:L348-L355 (line 106)**: The DEFAULT_CRON_JITTER_CONFIG snippet closely matches but comment formatting in the source has additional detail not shown.
- **cronTasks.ts:L108-L120 (line 467)**: The readCronTasks validation loop closely matches but the logForDebugging call formatting differs slightly.

## Uncited Source Files

- `src/tools/ScheduleCronTool/UI.tsx` — not cited in the chapter body.

## Verified Snippets

The following snippets match their source files verbatim or with only trivial whitespace differences:
- cronTasks.ts:L30-L70 (CronTask type)
- cron.ts:L10-L16 (CronFields type)
- cronTasks.ts:L315-L346 (CronJitterConfig type)
- cron.ts:L83-L101 (parseCronExpression)
- cronTasks.ts:L381-L398 (jitteredNextCronRunMs)
- cronTasks.ts:L421-L445 (oneShotJitteredNextCronRunMs)
- cronTasks.ts:L194-L219 (addCronTask)
- cronTasks.ts:L453-L458 (findMissedTasks)
- CronCreateTool.ts:L117-L121 (call method with kill switch)
- prompt.ts:L36-L45 (isKairosCronEnabled)
