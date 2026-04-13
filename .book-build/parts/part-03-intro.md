# Part III. The Tool System

An agent without tools is just a chatbot. The tool system is what transforms a language model into an actor that can read files, write code, search codebases, execute commands, and interact with the world. Part III covers cc's tool architecture from the base type through the dispatch pipeline to the seven concrete tool families.

The theme of Part III is the tool contract: how tools are defined, how they are dispatched safely, and what each tool family does. These seven chapters are grouped together because they form a self-contained subsystem -- the tool layer sits between the query loop (Part II) and the model, translating model requests into side effects with appropriate safety checks. Understanding this layer is essential because tool dispatch is the primary mechanism through which an agent affects the world, and every tool call passes through the same validation, permission, and hook pipeline.

The tool system embodies several of HER's core patterns. Pattern 11 (Single-Purpose Tool Design) is the foundational principle: each tool does one thing well, with a clear input schema and a deterministic output format. Pattern 9 (Progressive Tool Expansion) governs how tools are surfaced to the model -- not all at once, but on demand via ToolSearch. Pattern 12 (Deterministic Lifecycle Hooks) structures the pre-tool-use and post-tool-use hook execution that gates every dispatch. And Pattern 10 (Command Risk Classification) is implemented in the Bash tool's classifier pipeline, which assigns risk bands to shell commands before they execute.

Chapter 11 dissects the tool contract itself -- the Zod/JSONSchema input validation, the `call()` method, concurrency flags, deferral semantics, and max result size -- walking through the `buildTool` helper that generates the standardized tool wrapper. This chapter establishes the vocabulary for every subsequent tool chapter: what a tool definition looks like, how input is validated, and how results are normalized.

Chapter 12 traces the tool dispatch pipeline: how a requested tool call travels through `validateInput`, `canUseTool`, `runPreToolUseHooks`, `tool.call`, `runPostToolUseHooks`, and result normalization, with read-only parallelism and write serialization. This is the pipeline that every tool call follows, and understanding it is prerequisite for understanding the permission system (Part VI) and the hook system (Chapter 36 and Chapter 37).

Chapter 13 covers the file system tool family -- Read, Write, Edit, Glob, Grep, and NotebookEdit -- including safety checks, multi-edit semantics, image and PDF support, and the relationship to file-history snapshots. These are the most frequently used tools in cc, and their implementation reveals important design decisions about safety, idempotency, and result formatting.

Chapter 14 examines the Bash tool and its safety subsystem, including command classification, destructive pattern detection, mode validation, sandbox detection, and the PowerShell variant. The Bash tool is the highest-risk tool in cc's catalog, and its safety architecture is the most elaborate -- a multi-stage pipeline that classifies, validates, and optionally sandboxes every command before execution.

Chapter 15 explores the search and analysis tools -- GrepTool, LSPTool, and ToolSearchTool -- and explains progressive tool disclosure and why it matters for large catalogs. ToolSearch is the mechanism by which cc avoids tool explosion: instead of exposing all tools to the model at once, it surfaces them on demand when the model asks what tools are available.

Chapter 16 covers the web tools -- WebFetch and WebSearch -- including SSRF guard, caching, and how web access is gated, tying back to the prompt-injection threat model. Web tools are unique in that they fetch untrusted external content, making them a primary attack surface that requires dedicated defenses.

Chapter 17 examines the meta tools that structure sessions: TodoWrite for self-planning, AskUserQuestion for human-in-the-loop interrupts, Brief for summary generation, Config for settings changes, and SendMessage for teammate communication. These tools are not about performing work on the filesystem or the network; they are about structuring the agent's own process and communicating with the user and other agents.

By the end of this Part, you should understand the complete lifecycle of a tool call in cc: how it is validated, dispatched, executed, and post-processed; how each tool family works internally; and how safety and permission checks gate every side effect. This understanding is essential for Part IV (which covers the Agent tool), Part VI (which covers the permission system in depth), and Part X (which evaluates the tool system against HER's patterns and failure modes).

Return to the Table of Contents in the front matter for an overview of all ten parts and fifty-seven chapters.
