# Gaps Audit: Chapter 23 — Teammates and In-Process Collaboration

## Overview vs Synopsis Match

Synopsis: "tmux-based teammates, in-process teammate tasks, SendMessageTool, idle hooks."

The chapter covers SendMessageTool extensively and in-process teammate tasks well. However:
- **tmux-based teammates**: The chapter mentions tmux pane IDs in passing (handleShutdownApproval references `selfMember.tmuxPaneId`) but does not have a dedicated discussion of tmux-based teammates as a concept. The chapter focuses almost entirely on the in-process tier.
- **idle hooks**: The chapter mentions teammates entering an "Idle" state in the lifecycle diagram, and discusses auto-resume of stopped agents, but does not explicitly discuss "idle hooks" as a mechanism. The session hooks system is covered, but the specific idle-hook mechanism (hooks that fire when a teammate becomes idle) is not addressed.

## Source File Citation

Brief source files:
1. `src/tools/SendMessageTool/SendMessageTool.ts` — **Cited extensively** (primary source)
2. `src/tools/SendMessageTool/constants.ts` — **Not cited** (trivial: only contains `SEND_MESSAGE_TOOL_NAME = 'SendMessage'`)
3. `src/tools/SendMessageTool/prompt.ts` — **Not cited** (contains getPrompt() function with UDS_INBOX-gated content)
4. `src/tools/SendMessageTool/UI.tsx` — **Not cited** (contains render functions)
5. `src/utils/hooks/sessionHooks.ts` — **Cited extensively** (secondary source)

3 of 5 source files uncited. However, constants.ts is trivial (1 line), and prompt.ts and UI.tsx are supporting files. This is informational rather than a critical gap, but prompt.ts contains important behavioral documentation (the UDS_INBOX-gated prompt text) that would have enriched the chapter.

## Mandated Diagrams

1. (a) sequenceDiagram of a teammate message exchange — **Present** (message routing diagram)
2. (b) stateDiagram-v2 of teammate life (active/idle/shutdown) — **Present** (lifecycle diagram)

Both mandated diagrams are present.

## Minimum Counts

- Citations: 39 (minimum 6) — **Pass**
- Diagrams: 2 (minimum 2) — **Pass**
- Snippets: 8 (minimum 4) — **Pass**

## Top Source Files Without Snippets

Top 3 source files by centrality:
1. `src/tools/SendMessageTool/SendMessageTool.ts` — Has snippets (primary source)
2. `src/utils/hooks/sessionHooks.ts` — Has snippets
3. `src/tools/SendMessageTool/prompt.ts` — **No snippet** (third most central)

The prompt.ts file contains the UDS_INBOX-gated prompt text that documents cross-session messaging behavior. A snippet from this file would strengthen the chapter's coverage of the UDS addressing schemes.

## Uncovered Topics

1. **tmux-based teammates**: The synopsis mentions "tmux-based teammates" but the chapter does not discuss how tmux-based teammates differ from in-process teammates in terms of lifecycle, communication, and shutdown. The chapter touches on this in the shutdown section (process-based vs in-process) but does not give it dedicated coverage.

2. **Idle hooks mechanism**: The synopsis mentions "idle hooks" but the chapter does not describe any hook mechanism that fires specifically when a teammate transitions to idle state. The SessionHooks system is covered, but idle-specific hooks are not.

3. **Teammate spawning via TeamCreateTool**: The lifecycle diagram mentions "TeamCreateTool dispatches teammate" but the chapter does not discuss how teammates are initially spawned, the TeamCreateTool, or the team creation flow. This is relevant context for understanding the in-process collaboration lifecycle.

## Summary

- 3 uncited brief files (2 trivial, 1 substantive)
- 0 missing mandated diagrams
- 3 uncovered topics (tmux-based teammates, idle hooks, teammate spawning)
- 1 top file without a snippet (prompt.ts)
