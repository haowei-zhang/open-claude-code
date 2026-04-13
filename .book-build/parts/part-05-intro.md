# Part V. Context, Memory, and State

The hardest problem in long-running agent systems is not generating good responses -- it is maintaining coherence across thousands of turns, hundreds of tool calls, and multiple compaction cycles. Part V covers the systems that manage what the agent knows: messages, memory, compaction, session persistence, application state, and configuration.

The theme of Part V is context management across time. These seven chapters are grouped together because they address the same fundamental challenge: how does an agent that can only hold a finite context window maintain awareness of an arbitrarily long session? Each chapter covers a different layer of the solution, from the message model through persistent memory to the compaction hierarchy that rescues the context window when it fills.

Context management is the problem that distinguishes long-running agents from simple chatbots. A chatbot can afford to forget everything after each response. A long-running agent must remember what it did, what it learned, and what it promised to do -- not just for the current turn, but across an entire session that may span hours and thousands of interactions. HER identifies context rot (failure mode 6.1) as one of the most pervasive failure modes in production agents, and cc's response to this challenge is one of its most sophisticated subsystems.

Chapter 25 covers `src/utils/messages.ts` and `src/types/message.ts` -- the message types, `<history_snip>` markers, compact boundaries, attachments, and content replacement. Messages are the fundamental data structure of the conversation, and understanding their representation is prerequisite for understanding compaction, memory extraction, and session persistence.

Chapter 26 explores the memdir architecture -- the MEMORY.md index, per-memory markdown files, memory types (user, feedback, project, reference), age tracking, scanning, and relevance retrieval. Memdir is cc's implementation of HER's Pattern 3 (Tiered Memory): a persistent, filesystem-based memory store that survives across sessions and provides fast relevance retrieval for the current query.

Chapter 27 examines the SessionMemory service and memory extraction pipelines, the prompts that drive extraction, and feature gates. This chapter covers how cc decides what to remember from a session: the extraction pipeline that runs after each session, the prompts that identify salient information, and the feature gates that control when extraction fires.

Chapter 28 traces the compaction hierarchy through its five stages: `<history_snip>` replacement, Microcompact, Context Collapse, Autocompact, and Hard Reset -- including triggers, preservation semantics, and discard semantics. The compaction hierarchy is cc's most distinctive architectural feature and its implementation of HER's Pattern 5 (Progressive Context Compaction). Understanding it is essential for understanding how cc handles the inevitable context overflow that occurs in long sessions.

Chapter 29 covers session persistence and resume -- JSONL session storage, entry types, tombstones, head and tail reading, and the resume flow. Session persistence is what allows a cc process to be restarted without losing its conversational context. This chapter covers the storage format, the indexing strategy, and the resume flow that reconstructs a session from its persisted state.

Chapter 30 dissects the AppState store -- its Redux-like shape, reducers, subscribers, and team context. AppState is the global state management layer that tracks the current session, tool results, permission decisions, and other runtime state. It provides the reactive foundation that connects the query loop to the terminal UI.

Chapter 31 examines the settings cascade (org to user to project to directory to environment to flags), managed settings for enterprise, and CLAUDE.md discovery. The settings cascade is cc's implementation of HER's Configuration Surfaces principle: a six-layer precedence system that allows settings to be specified at multiple levels of granularity, from organizational policy down to per-command flags.

By the end of this Part, you should understand how cc represents and manages the full lifecycle of conversational context: how messages are stored and compacted, how memories persist across sessions, how the compaction hierarchy rescues overflowing contexts, how sessions survive process restarts, how application state is managed, and how configuration cascades through multiple layers. This understanding is essential for Part IV (which covers dream tasks that write to the memory system), Part VII (which covers the REPL that renders state), and Part X (which evaluates cc's context management against HER's patterns and failure modes).

Return to the Table of Contents in the front matter for an overview of all ten parts and fifty-seven chapters.
