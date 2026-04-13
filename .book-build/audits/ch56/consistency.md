# Consistency Audit Report - Chapter 56

## Term Conflict Check

Scanning chapter for terminology registry conflicts:

1. "tool" - Used consistently with registry definition. No conflicts.
2. "subagent" - Used consistently (not "sub-agent"). No conflicts.
3. "fork" - Used correctly in context of subagent dispatch. No conflicts.
4. "task" - Used consistently with registry definition. No conflicts.
5. "hook" - Used correctly (PreToolUse, PostToolUse). No conflicts.
6. "permission mode" - Used correctly referencing default/plan/acceptEdits/bypassPermissions/dontAsk/auto. No conflicts.
7. "classifier" - Used correctly in reference to command risk classification. No conflicts.
8. "compaction" - Used correctly referencing the five-stage hierarchy. No conflicts.
9. "autocompact" - Used correctly referencing the automatic compaction trigger. No conflicts.
10. "circuit breaker" - Used correctly referencing MAX_CONSECUTIVE_AUTOCOMPACT_FAILURES. No conflicts.
11. "query loop" - Used correctly referencing the async generator in src/query.ts. No conflicts.
12. "back-pressure" - Used correctly referencing resource-limit throttling. No conflicts.
13. "observation masking" - Used correctly referencing the cost-reduction technique. No conflicts.
14. "progressive tool expansion" - Referenced via ToolSearch/deferred tool loading. No conflicts.
15. "deferred tool" - Referenced correctly. No conflicts.
16. "analytics sink" - Used correctly referencing AnalyticsSink interface. No conflicts.
17. "cost metering" - Referenced correctly in context of cost tracking. No conflicts.

## Term Conflict Found

- "task budget" - The terminology registry defines "task budget" as "An API-level budget for the whole agentic turn (output_config.task_budget), distinct from the client-side token budget." The chapter introduces a DIFFERENT concept of "TaskBudget" as a per-task cost cap with capUSD/spentUSD/state fields. This is a conflict: the chapter's proposed `TaskBudget` interface is a cost-control mechanism, while the registry's "task budget" refers to the API-level output budget. These are fundamentally different concepts.

## Voice Drift

The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected. The chapter naturally has a more prescriptive tone (it's a roadmap), but the prescriptive statements are grounded in citations and code evidence.

## Proposed New Terms

1. "quality-gates pipeline" - A deterministic checkpoint pipeline wired into the query loop that evaluates tool results with lint/type-check/test gates and routes verdicts to pass/fail/escalate actions.
2. "loop detector" - A sliding-window monitor that flags identical tool calls within a configurable time window, preventing infinite retry loops.
3. "spend-rate monitor" - A rolling-average anomaly detector that fires alerts when both cost-per-minute and tokens-per-minute exceed 2x their respective rolling averages.
4. "escalation record" - A context-rich payload attached to human-in-the-loop prompts carrying the agent's confidence, intent, prior attempts, options, and recommended action.
5. "trace span" - An OpenTelemetry-compatible observability record linking tool dispatches, subagent spawns, and model invocations into a causal trace tree.
