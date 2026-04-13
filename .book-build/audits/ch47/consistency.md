# Consistency Audit Report — Chapter 47

## Summary

Verdict: **pass**

Zero term conflicts. No voice drift detected.

## Term Usage Check

All registered terms used in this chapter are used with their canonical definitions:

- **tool**: Used correctly for CronCreate, CronDelete, CronList
- **task**: Used correctly for CronTask durable unit of work
- **feature gate**: Used correctly for AGENT_TRIGGERS and GrowthBook gates
- **session**: Used correctly for session-only vs durable distinction
- **hook**: Used correctly in context of hook events
- **subagent**: Not used in this chapter (correct — no subagent discussion)

## Voice Check

The chapter is written in present tense, descriptive, cite-heavy style — consistent with the house style. No voice drift detected.

## Proposed New Terms

5 new terms proposed for the terminology registry:

1. **cron jitter** — Deterministic per-task delay applied to cron fire times to prevent thundering-herd conditions, derived from the task ID for reproducibility across restarts.
2. **one-shot cron task** — A cron task that fires once and is auto-deleted, as opposed to a recurring task which reschedules after firing.
3. **session-only cron** — A cron task held in process memory that never touches the filesystem and dies with the process when durable is false.
4. **missed-task detection** — The startup check that identifies cron tasks whose scheduled fire time passed while the REPL was closed, surfacing them for catch-up.
5. **dynamic-pacing loop** — A /loop mode using ScheduleWakeup with simple relative delays instead of cron expressions, allowing the loop to adjust its pace based on task progress and prompt cache warmth.
