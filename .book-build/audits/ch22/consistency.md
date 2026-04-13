# Consistency Audit: Chapter 22 — Tasks: A Durable Unit of Work

## Terminology Consistency

The chapter uses registered terms correctly:
- **task**: Used consistently with the canonical definition (durable unit of work with status transitions)
- **subagent**: Used correctly when discussing LocalAgentTask
- **hook**: Used correctly when discussing TaskCreated and TaskCompleted hooks
- **permission mode**: Not used in this chapter (not relevant)
- **compaction**: Not used in this chapter
- **verification nudge**: Referenced correctly in context of the VERIFICATION_AGENT feature

No term conflicts found. All registered terms are used with their canonical definitions.

## Voice Consistency

The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **high water mark** — A persistent counter file (.highwatermark) that stores the maximum task ID ever assigned, preventing ID reuse after deletion or reset operations.
2. **task list identity** — The mechanism by which getTaskListId() determines which task list an agent operates on, using a priority chain: environment variable, teammate context, team name, leader team name, or session ID.
3. **task assignment notification** — A JSON message sent via the teammate mailbox when a task's owner changes, ensuring the newly assigned teammate learns about the work without polling the task list.
4. **reference sweeping** — The O(N) operation performed after task deletion that removes references to the deleted task from all other tasks' blocks and blockedBy arrays, maintaining referential integrity.
5. **verification nudge** — The advisory signal in TaskUpdateTool that fires when the main-thread agent completes 3+ tasks without including a verification step, appending a reminder to spawn the verification subagent.

## Verdict: pass
