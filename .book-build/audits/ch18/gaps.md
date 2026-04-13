# Gaps Audit: Chapter 18

## Summary

Verdict: **revise**

## Brief Coverage

The chapter's synopsis is: "AgentTool surface. Inputs (description, prompt, subagent_type, model, run_in_background, team_name, mode, isolation, cwd), validation, output return." The chapter covers all listed input fields extensively.

## Source File Coverage

- `src/tools/AgentTool/AgentTool.tsx` - cited extensively (multiple snippets). No gap.

## Diagram Requirements

Brief requires:
- (a) classDiagram of AgentTool input schema - **Not present**. Chapter has a classDiagram of the output schema instead. The input schema is described in prose and code but lacks a visual diagram.
- (b) flowchart of AgentTool input validation and dispatch selection - **Present** (dispatch selection flowchart).

Missing 1 of 2 mandated diagrams by topic.

## Metrics

- Citation count: 15 (minimum: 6) - PASS
- Diagram count: 2 (minimum: 2) - PASS
- Snippet count: 15 (minimum: 4) - PASS
- Top files without snippets: none

## Uncovered Topics

1. The brief mandates a classDiagram of the AgentTool **input** schema; the chapter provides a classDiagram of the **output** schema instead. This is a gap in diagram topic coverage.
