# Part III: The Tool System

Tools are the hands of an agent. A language model without tools can reason about the world but cannot act in it. A harness without a well-designed tool system can route prompts but cannot translate model intent into filesystem changes, shell commands, web requests, or structured interactions with the user. Part III examines the complete tool architecture of cc: the base contract that every tool satisfies, the dispatch pipeline that governs execution, and the individual tool families that give the agent its capabilities.

These seven chapters belong together because they span a single conceptual stack. At the bottom is the abstract tool contract and the `buildTool` helper -- the type system and runtime primitives that every tool must conform to. Above that sits the dispatch pipeline, which enforces validation, permission checks, lifecycle hooks, concurrency rules, and result normalization for every tool call regardless of type. And above that are the concrete tool families: file operations, shell execution, search and code analysis, web access, and the meta-tools that structure sessions themselves. Reading these chapters in order builds a layered understanding, from the generic to the specific, that mirrors how the codebase itself is organized.

Chapter 11, "Anatomy of a Tool: `Tool.ts`, `buildTool`, and the Base Type," defines the tool contract -- Zod and JSON Schema input validation, the `call()` method, concurrency flags, deferral semantics, and maximum result size. It walks through the `buildTool` helper that most tools use to construct their implementation.

Chapter 12, "Tool Dispatch Pipeline," traces the path of a tool call from the model's output through `validateInput`, `canUseTool`, `runPreToolUseHooks`, `tool.call`, `runPostToolUseHooks`, and result normalization. The chapter covers read-only parallelism, write serialization, and the deferral and concurrency scheduling that keep the agent from deadlocking itself.

Chapter 13, "File System Tools: Read, Write, Edit, Glob, Grep, Notebook," covers the file-tool family and its safety checks, multi-edit semantics, support for images, PDFs, and Jupyter notebooks, and the relationship between file operations and the file-history snapshot system.

Chapter 14, "The Bash Tool, Classifiers, and Sandboxing," examines BashTool and its safety subsystem: command classification into risk bands, destructive pattern detection, mode validation, sandbox detection, and the PowerShell variant for Windows environments.

Chapter 15, "Search, LSP, and Code Analysis Tools," covers GrepTool, LSPTool, and ToolSearchTool. The chapter explains progressive tool disclosure -- the strategy of deferring tool registration until the model needs them -- and why this matters for large tool catalogs that would otherwise saturate the prompt.

Chapter 16, "Web Tools: `WebFetch`, `WebSearch`, and SSRF Defense," details the web tool implementations, the SSRF guard that prevents the agent from reaching internal network addresses, the caching layer, and how web access is gated. This chapter connects directly to the prompt-injection threat model discussed later in Part IX.

Chapter 17, "TodoWrite, AskUserQuestion, Brief, Config, SendMessage," covers the meta-tools that structure sessions: TodoWrite for self-planning and task tracking, AskUserQuestion for human-in-the-loop interruptions, Brief for context compaction, Config for runtime setting changes, and SendMessage for inter-agent communication.

By the end of Part III, the reader should understand the full tool architecture: what a tool is at the type level, how it is dispatched and guarded at runtime, and what each concrete tool family does and why. This knowledge is a prerequisite for Part IV, where tools become the building blocks of sub-agents, and for Part VI, where the permission and hook systems impose additional constraints on tool execution.

The table of contents in the front matter provides the full chapter listing and page numbers for the entire book.
