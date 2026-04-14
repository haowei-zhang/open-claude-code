# Part 5: Context, Memory, and State

An agent without memory is condemned to live in an eternal present. Every session starts from scratch; every hard-won insight evaporates when the loop ends. An agent without state management cannot resume, cannot compact, cannot coordinate. And an agent whose context window grows without bound eventually drowns in its own history. Part 5 addresses the three intertwined problems that determine whether an agent is ephemeral or enduring: how messages flow through the conversation, how knowledge persists across sessions, how context is rescued when it overflows, and how all of this is bound together by a centralized store and a cascading configuration system.

This part groups seven chapters around a single theme: the architecture of remembering, forgetting, and recovering. The chapters proceed from the atomic unit -- the message -- outward through persistent memory, compaction, session storage, application state, and the configuration cascade that governs all of it.

Chapter 25, "Messages and the Conversation Model," examines src/utils/messages.ts and the message type system: the distinct message types, the `<history_snip>` boundary that marks compacted regions, attachment handling, and the content replacement semantics that allow the system to rewrite its own history.

Chapter 26, "Memdir: Tiered Memory on Disk," introduces the memdir architecture -- a filesystem-backed memory store with a MEMORY.md index and per-memory Markdown files. This chapter covers the four memory types (user, feedback, project, reference), age tracking, memory scanning, and the relevance retrieval algorithm that surfaces the right memories at query time.

Chapter 27, "Session Memory and Memory Extraction," describes the services/SessionMemory/ pipeline and the memory extraction prompts that distill what matters from a session, promoting ephemeral context into durable memdir entries behind feature gates.

Chapter 28, "Compaction Hierarchy: Five Stages of Context Rescue," maps the escalation ladder that cc climbs when context pressure mounts: `<history_snip>` summarization, microcompaction, context collapse, autocompaction, and hard reset. Each stage has distinct triggers, preservation semantics, and discard policies.

Chapter 29, "Session Persistence and Resume," covers the JSONL session format, entry types, tombstone records, head/tail reading strategies, and the full resume flow that reanimates a session from disk after a crash or deliberate interruption.

Chapter 30, "AppState: The Redux-like Store," opens the centralized state container -- AppState.tsx and AppStateStore.ts -- explaining the store shape, reducer patterns, subscriber notification, and how teamContext threads through for multi-agent coordination.

Chapter 31, "CLAUDE.md, Settings Cascade, and Managed Settings," closes the part with the loading order that determines which instruction wins when conflicts arise: org, user, project, directory, environment, and CLI flags, plus the managed settings system that lets enterprise administrators lock down configuration.

These chapters belong together because they form the substrate of agent continuity. Messages are the atoms; memdir is the long-term repository; extraction bridges the two; compaction prevents overflow; persistence enables resume; AppState provides the live backbone; and the settings cascade determines whose instructions prevail. Each chapter depends on the ones before it, and together they answer the question: how does an agent that runs on a finite context window maintain coherence across sessions, across compactions, and across conflicting configuration sources?

By the end of this part, the reader should understand the full lifecycle of context in cc -- from a fresh message arriving in the conversation, through extraction into persistent memory, through compaction when the window fills, through persistence to disk, and back into a resumed session. The reader should also grasp why cc's compaction hierarchy is not a single algorithm but a staged escalation, and why the settings cascade is not a simple override chain but a carefully ordered sequence that accounts for enterprise control, user preference, project convention, and runtime flags.

Return to the table of contents in the front matter for the full chapter listing and cross-references to HER patterns and failure modes.
