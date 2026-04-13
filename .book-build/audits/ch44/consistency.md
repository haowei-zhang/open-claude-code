# Consistency Audit: Chapter 44

## Term Conflicts
No term conflicts found. All registered terms are used with their canonical definitions.

## Voice Drift
No voice drift detected. The chapter is written in present tense, descriptive, cite-heavy style consistent with house style.

## Proposed New Terms
1. **bridge**: A bidirectional transport layer connecting cc's in-process agent loop to external orchestrators (IDE extensions, web clients, SDK daemons), implementing message translation, session lifecycle management, OAuth token refresh, and permission proxying.
2. **work-secret authentication**: A per-session authentication mechanism where each spawned cc session is bound to a unique work secret generated at spawn time, preventing unauthorized clients from connecting to or injecting messages into a running session.
3. **capacity wake signal**: A signaling mechanism that wakes the work-polling loop immediately when a session completes, reducing latency between session completion and new work assignment from the polling interval to near-zero.
