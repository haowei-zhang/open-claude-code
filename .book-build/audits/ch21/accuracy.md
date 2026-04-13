# Accuracy Audit: Chapter 21

## Summary

Chapter 21 cites `src/coordinator/coordinatorMode.ts` extensively. The file exists and is 369 LOC as claimed. Of 21 citations, 19 verify correctly.

## Issues

1. **Snippet drift at L174**: The anti-pattern/good-example snippet at `src/coordinator/coordinatorMode.ts:L259-L268` uses literal `AgentTool` instead of the template-interpolated `${AGENT_TOOL_NAME}` that appears in the actual source. The line range is slightly off.

2. **Snippet drift at L193**: The XML notification format snippet at `src/coordinator/coordinatorMode.ts:L143-L163` is presented as TypeScript comments wrapping XML, but the actual source contains raw XML inside a template literal. The line range is slightly off.

## Verdict

Two snippet-drift issues, zero hallucinated snippets, snippet count meets minimum. Verdict: **revise**.
