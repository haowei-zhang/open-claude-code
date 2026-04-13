# Gaps Audit Report - Chapter 56

## Overview vs Synopsis Verification

Brief synopsis: "Concrete roadmap for a team building on top of the cc codebase: what to keep, what to rewrite, what to add."

Chapter overview matches: The chapter presents a three-track roadmap (Keep, Rewrite, Add) and is structured around exactly this framework. MATCH.

## Source File Citations

This is a meta-chapter with no source_files in the brief. The chapter references source files as evidence for gaps:
- `src/cost-tracker.ts` - CITED
- `src/tools/AskUserQuestionTool/AskUserQuestionTool.tsx` - CITED
- `src/services/analytics/index.ts` - CITED
- `src/services/compact/autoCompact.ts` - CITED
- `src/hooks/useCanUseTool.tsx` - CITED
- `src/utils/hooks/ssrfGuard.ts` - CITED (in text)
- `src/utils/tasks.ts` - CITED (in text)
- `src/bootstrap/state.ts` - CITED (in text)
- `src/services/analytics/growthbook.ts` - CITED (in text)

No uncited brief files (brief has no source_files).

## Mandated Diagrams

Brief requires:
- (a) classDiagram of proposed additions - FOUND (classDiagram with Existing/TaskBudget/EscalationRecord/TraceSpan/QualityGatePipeline/LoopDetector/SpendRateMonitor/AgentDashboard)
- (b) flowchart of the new quality-gates pipeline - FOUND (flowchart TD showing quality-gates pipeline)

All mandated diagrams present.

## Minimum Counts

- Citation count: 10 (well above minimum of 6)
- Diagram count: 2 (meets minimum of 2)
- Snippet count: 12 (well above minimum of 4)

## Top Source Files Without Snippets

This chapter has no source_files in the brief, so this check is N/A. However, the chapter does cite snippets from the key referenced files (cost-tracker.ts, AskUserQuestionTool.tsx, analytics/index.ts, autoCompact.ts, useCanUseTool.tsx).

## Uncovered Topics

1. The chapter does not discuss how the proposed quality-gates pipeline would interact with the existing `shouldAutoCompact` function. When a quality gate fails and triggers a retry, the retry loop could approach the autocompact threshold. This interaction should be addressed.

2. The chapter mentions the `pollable` flag for tools as an extension to the `readOnly` classifier but does not discuss how this would be implemented in the existing `commandSemantics.ts` or `readOnlyValidation.ts` modules.

3. The chapter does not discuss the migration path for existing sessions that are in progress when new subsystems (TaskBudget, TraceSpan) are deployed. Session compatibility is important for a long-running agent.

These are minor gaps that do not fundamentally undermine the chapter's roadmap.
