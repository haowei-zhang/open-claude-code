# Deep Research and Development Guide of CC Source Code

Based on repository SHA `a371abbe75ffa0d0a3c92290e2bbf56a7ef54367`

## Preface

In early 2026, a leaked snapshot of the cc (Claude Code) source codebase began circulating among engineers working on autonomous agent systems. The leak was not a breach of proprietary secrets in the conventional sense -- cc had always been a publicly available npm package -- but the full, unminified source tree, complete with comments, feature flags, and internal tooling, offered something the published package did not: a transparent view into the engineering decisions that make a long-running agent harness work. Around the same time, the Harness Engineering Report (HER) was published, providing a rigorous analytical framework for evaluating agent infrastructure. This book was born from the intersection of those two events.

The cc codebase is the most mature public example of a long-running agent harness ever made available for study. At 4,683 lines in its main entry point alone, with over forty registered tools, a five-stage compaction hierarchy, and a multi-path dispatcher that routes a dozen different execution modes before the first API call, cc is not a thin wrapper around an API. It is an engineering artifact of considerable depth, encoding hard-won lessons about what it takes to make a language model productive over hours of unattended work. This book exists to make those lessons explicit, traceable, and reusable.

The thesis of this book is straightforward: cc is both a specimen and a template. By reading its source carefully, engineers building their own autonomous systems can learn what a production-grade harness actually requires. Not the toy demos of a weekend project, but the permission systems that prevent catastrophic file deletions, the context budgets that keep the model from running out of tokens mid-task, the lifecycle hooks that let organizations inject policy without modifying the core, and the failure recovery paths that distinguish a trustworthy agent from a fragile one.

The discipline of harness engineering, as articulated by Mitchell Hashimoto and elaborated in the HER, builds on two prior layers of practice. Prompt engineering, which emerged in the GPT-3 era around 2020, focused on crafting instructions -- telling the model what to do. Context engineering, which emerged in the RAG era around 2023, focused on managing information flow -- giving the model what to know. Harness engineering, emerging in the agent era around 2025, focuses on infrastructure and environments -- giving the model where to work. These are nested layers, not sequential eras: a harness engineer must also be a competent context engineer and prompt engineer. Each layer adds scope without eliminating the previous one.

This book is written for engineers building long-running agent harnesses. If you are designing the tool dispatch pipeline for a coding agent, implementing a permission model that balances autonomy with safety, or debugging why your agent's context window collapses after thirty minutes, the chapters that follow contain direct, source-verified answers. The intended audience is not academic researchers looking for formal proofs, nor managers seeking executive summaries, but working engineers who need to understand, modify, or replicate the systems that make cc reliable.

Three reading paths are available. The linear path reads Parts I through X in order, building understanding from foundations through the runtime spine, tool system, multi-agent dispatch, context management, safety infrastructure, interaction surfaces, long-running work patterns, observability, and synthesis. This is the path for someone building a harness from scratch who wants the full picture. The subsystem-focused path jumps directly to the part that covers the subsystem under construction: Part III for the tool system, Part IV for multi-agent orchestration, Part V for memory and compaction, Part VI for permissions and hooks. Each part opens with sufficient context to be read independently, though deep comprehension benefits from the linear path. The pattern-focused path, for readers already familiar with cc's architecture, maps the twelve HER patterns and seventeen failure modes to their concrete implementations using the cross-reference tables in Part X, then traces backward into the relevant chapters for evidence.

The origin of this book in a leaked codebase is worth addressing directly. The cc source was not obtained through authorized channels, and no endorsement from Anthropic is implied. However, the code was already public in compiled form; the leak merely removed the minification barrier. The analysis in this book relies on the same code that any npm install would deliver, just with the variable names intact. Where the analysis touches on security-sensitive mechanisms -- the permission model, the SSRF guard, the sandbox detection -- the purpose is educational: to show how one production system solves these problems, not to provide a roadmap for circumventing them. Engineers building trustworthy systems need to understand both the defenses and their limitations, and this book aims to serve that need honestly.

The HER provides the empirical and theoretical substrate for this book. Every chapter cross-references specific HER sections, patterns, and failure modes. When a chapter claims that cc implements Progressive Tool Expansion (HER Pattern 9), the claim is backed by file paths, line numbers, and quoted code. When a chapter identifies a gap where cc lacks a defense against a known failure mode, that gap is documented with equal rigor. The goal is not hagiography but engineering clarity: here is what works, here is what does not, and here is the code that proves it.

## How to Read This Book

### Citation format

Every factual claim about the cc codebase is cited inline using one of two formats:

- **Source citations** follow the pattern `src/path/file.ts:Lnnn`, pointing to a specific file and line number in the repository. For ranges, the format is `src/path/file.ts:Lnnn-Lmmm`. These citations are stable against the repository SHA listed in the byline above.
- **HER citations** follow the pattern `HER Snn` or `HER Snn Pattern N`, pointing to a specific section or pattern within the Harness Engineering Report. The HER is a companion document to this book.

When a citation appears after a factual claim, the referenced source contains the evidence for that claim. When a citation appears before a code block, the referenced source is the origin of the quoted code. All code blocks in this book are verbatim quotations from the source, preserving original indentation, comments, and variable names.

### Diagram conventions

This book uses five types of Mermaid diagrams, each serving a distinct purpose:

1. **flowchart** -- Control flow and decision trees. Used when the chapter traces a sequence of conditional branches (e.g., permission rule evaluation, prompt assembly layers).
2. **sequenceDiagram** -- Temporal interactions between components. Used when the chapter traces a request or event flowing through multiple modules in time order (e.g., tool dispatch, streaming request lifecycle).
3. **stateDiagram-v2** -- State machines and lifecycle transitions. Used when the chapter describes a finite set of states and the events that cause transitions between them (e.g., boot state machine, task status transitions, compaction triggers).
4. **classDiagram** -- Type hierarchies and structural relationships. Used when the chapter maps the fields and methods of a type or the inheritance/composition relationships between types (e.g., Tool interface, message types, hook schema).
5. **erDiagram** -- Persistent storage layouts and entity relationships. Used when the chapter describes data that is serialized to disk or stored in a structured format (e.g., memdir storage, session JSONL layout, worktree directory structure).

Each diagram is labeled with its type and topic. Diagrams are not decorative; they encode the same information as the surrounding prose in a format optimized for visual parsing. When the prose and a diagram appear to conflict, the prose takes precedence, and the diagram should be treated as a simplified view.

### Chapter structure

Every chapter follows the same six-section structure:

1. **Overview** -- A narrative introduction that states the chapter's thesis, identifies the key modules, and previews the chapter's arc. Typically 500-800 words.
2. **Data structures and contracts** -- The types, interfaces, and schemas that define the chapter's subject. Every subsequent section depends on the definitions established here.
3. **Implementation walkthrough** -- A line-by-line or function-by-function trace through the source code, following the execution path from entry to exit.
4. **Cross-reference to HER** -- A mapping between the chapter's subject and the relevant HER patterns, failure modes, best practices, or reference architecture layers. Each mapping includes the HER citation and an assessment of how closely cc's implementation matches the HER recommendation.
5. **Gaps and divergences** -- Where cc's implementation falls short of the HER recommendation, or where it takes a different approach with different tradeoffs. This section is deliberately critical.
6. **Summary** -- A concise recap of the chapter's key findings, suitable for scanning.

This six-section structure is mandatory for every chapter. If a section would be empty (e.g., no gaps exist), the section header is still present with a brief note explaining why the section is vacuous, rather than being omitted entirely.

### Cross-reference notation

Cross-references between chapters use the format "Chapter N" or "Chapter N, Section X.Y." Cross-references to the HER use the format "HER Snn" or "HER Snn.PatternN." When a cross-reference appears in parentheses, it is a supplementary pointer; the surrounding text should be comprehensible without following it. When a cross-reference appears as the subject of a sentence, it is a required dependency; the reader should follow it before continuing.

The index at the back of the book provides a reverse mapping: for each key term, the pages and chapters where it appears. The concordance appendix provides a complete listing of every source file cited, with the chapters that cite it.

## Table of Contents

- **Part I. Foundations and Framing**
  - Chapter 1. Why This Book Exists: The Harness Engineering Moment
  - Chapter 2. Book Architecture, Reading Paths, and Citation Conventions
  - Chapter 3. A Guided Tour of the Repository
  - Chapter 4. TypeScript, Bun, React, Ink: The Unusual Runtime Stack

- **Part II. Entry, Bootstrap, and the Runtime Spine**
  - Chapter 5. Bootstrap: From `bun run` to Running Loop
  - Chapter 6. `main.tsx` and the Command Router
  - Chapter 7. The Query Loop: Heartbeat of the Agent
  - Chapter 8. Talking to Anthropic: `services/api/claude.ts` and Streaming
  - Chapter 9. System Prompts and Prompt Assembly
  - Chapter 10. Token Budgets, Effort, and Fast Mode

- **Part III. The Tool System**
  - Chapter 11. Anatomy of a Tool: `Tool.ts`, `buildTool`, and the Base Type
  - Chapter 12. Tool Dispatch Pipeline
  - Chapter 13. File System Tools: Read, Write, Edit, Glob, Grep, Notebook
  - Chapter 14. The Bash Tool, Classifiers, and Sandboxing
  - Chapter 15. Search, LSP, and Code Analysis Tools
  - Chapter 16. Web Tools: `WebFetch`, `WebSearch`, and SSRF Defense
  - Chapter 17. TodoWrite, AskUserQuestion, Brief, Config, SendMessage

- **Part IV. Sub-Agents, Tasks, and Multi-Agent Dispatch**
  - Chapter 18. The Agent Tool: Entry, Inputs, and Lifecycle
  - Chapter 19. Sync vs Fork vs Remote: The Execution Modes
  - Chapter 20. Agent Definitions: Loading, Frontmatter, and Built-Ins
  - Chapter 21. Multi-Agent Coordinator: The Swarm Layer
  - Chapter 22. Tasks: A Durable Unit of Work
  - Chapter 23. Teammates and In-Process Collaboration
  - Chapter 24. Dream Tasks: Background Consolidation

- **Part V. Context, Memory, and State**
  - Chapter 25. Messages and the Conversation Model
  - Chapter 26. Memdir: Tiered Memory on Disk
  - Chapter 27. Session Memory and Memory Extraction
  - Chapter 28. Compaction Hierarchy: Five Stages of Context Rescue
  - Chapter 29. Session Persistence and Resume
  - Chapter 30. AppState: The Redux-like Store
  - Chapter 31. CLAUDE.md, Settings Cascade, and Managed Settings

- **Part VI. Safety, Permissions, and Hooks**
  - Chapter 32. The Permission Model: Modes and Rules
  - Chapter 33. Bash Classifier and YOLO Scoring
  - Chapter 34. Filesystem Permissions and Path Guards
  - Chapter 35. `useCanUseTool`: The Engine Hook
  - Chapter 36. The Hook Schema and Lifecycle Events
  - Chapter 37. Hook Execution: Command, Prompt, HTTP, Agent, Function

- **Part VII. Interaction Surfaces: Skills, Slash, MCP, UX**
  - Chapter 38. Skills: Discovery, Frontmatter, Progressive Disclosure
  - Chapter 39. Slash Commands: 90+ Commands and the Registry
  - Chapter 40. MCP: Clients, Transports, and Lifecycle
  - Chapter 41. MCP Tools: Passthrough Permissions and Deferred Loading
  - Chapter 42. The Ink Renderer and Terminal Engine
  - Chapter 43. REPL, PromptInput, Typeahead, Voice, Keybindings
  - Chapter 44. Bridges: VS Code, JetBrains, and Web

- **Part VIII. Long-Running Work**
  - Chapter 45. Plan Mode V2: The Five-Phase Model
  - Chapter 46. Worktrees: Isolated Parallel Branches
  - Chapter 47. Cron, Schedule, Loop, and Wakeup
  - Chapter 48. Remote Agents and CCR Integration
  - Chapter 49. KAIROS Assistant, Buddy, and Experimental Layers

- **Part IX. Observability, Cost, and Security**
  - Chapter 50. Analytics, Cost Tracking, and GrowthBook
  - Chapter 51. Debug Logs, Diagnostics, and Doctor
  - Chapter 52. Threat Model: CC Through the Lens of HER Section 12

- **Part X. Synthesis and Future Development**
  - Chapter 53. The 12 HER Patterns Mapped to CC
  - Chapter 54. The 17 Failure Modes: How CC Defends (or Doesn't)
  - Chapter 55. Reference Architecture: Building a Trustworthy Long-Running Agent
  - Chapter 56. What To Build Next: An Opinionated Roadmap
  - Chapter 57. Closing: Lessons for the Harness Engineering Discipline

- **Appendix A. Glossary**
- **Appendix B. Bibliography**
- **Appendix C. Concordance**
