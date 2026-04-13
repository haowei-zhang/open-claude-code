# Consistency Audit: Chapter 23 — Teammates and In-Process Collaboration

## Term Conflict Check

Checked all terminology.json terms against chapter usage:

1. **tool** — Used correctly (refers to SendMessageTool, etc.)
2. **subagent** — Used correctly throughout
3. **fork** — Used correctly (contrasted with in-process teammates)
4. **task** — Used correctly (teammates are tasks in the task registry)
5. **hook** — Used correctly (session hooks, function hooks, command hooks)
6. **query loop** — Used correctly (teammate's query loop exits gracefully)
7. **session** — Used correctly
8. **dispatch pipeline** — Not used; chapter references "tool dispatch pipeline" correctly in context
9. **structured message protocol** — Chapter uses "structured messages" which aligns with the terminology.json definition
10. **safety-check decision reason** — Chapter references `decisionReason.type === 'safetyCheck'` and `classifierApprovable: false` which aligns with the terminology definition
11. **auto-resume on send** — Chapter describes this pattern correctly matching the terminology definition
12. **agent definition** — Not directly referenced
13. **coordinator mode** — Referenced in context of team lead, correct usage

### Conflicts Found

1. **"sub-agent" vs "subagent"**: Chapter uses "sub-agents" in passing reference to "forked subagents of Chapter 19". The terminology registry canonical form is "subagent" (no hyphen). This is a term conflict.

2. **"team lead" vs "TEAM_LEAD_NAME"**: The chapter refers to "team lead" in prose, which is fine as a natural language rendering of the concept. No conflict.

## Voice Drift Check

The chapter is written in present tense, descriptive style, with code citations. This matches the house style ("present tense, descriptive, cite-heavy"). No voice drift detected.

## Proposed New Terms

1. **agent name registry** — "A Map in AppState that maps agent names to their task IDs, enabling SendMessageTool to address teammates by human-readable name rather than by task ID."

2. **mailbox delivery** — "A file-based message delivery mechanism for inter-teammate communication that persists across process restarts, used as a fallback when in-process queue delivery is not possible."

3. **queue-and-drain pattern** — "A message delivery pattern where incoming messages are queued in a pendingMessages array and drained at tool-round boundaries, preventing race conditions from mid-execution message injection."

4. **permission mode inheritance** — "The mechanism by which a teammate transitioning from plan mode to implementation inherits the team lead's permission mode, with the exception that plan mode is replaced by default mode to prevent accidental write restrictions."

5. **in-process vs process-based shutdown** — "The distinction between aborting an in-process teammate's AbortController (shared process) versus calling gracefulShutdown() for a process-based teammate (separate process), necessary because in-process teammates cannot be terminated by killing the process."
