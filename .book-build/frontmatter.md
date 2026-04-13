# Deep Research and Development Guide of CC Source Code

Based on repository SHA `a371abbe75ffa0d0a3c92290e2bbf56a7ef54367`

## Preface

This book exists because of a convergence that few people expected. In early 2025, the full source code of Claude Code -- Anthropic's terminal-based agentic coding assistant -- was leaked to the public. Around the same time, the Harness Engineering Report (HER) was published, documenting twelve design patterns, seventeen failure modes, and twenty best practices for building long-running autonomous agent systems. For the first time, engineers could study not just the theory of harness engineering but a complete, production-grade implementation of it.

That intersection is what this book explores. Claude Code (referred to throughout as "cc") is the most mature public example of a long-running agent harness. It is not a toy demo or a research prototype; it is a shipping product that processes millions of queries, manages concurrent sub-agents, enforces multi-layered security policies, and runs for hours across complex multi-step tasks. Every design decision in its codebase -- from the five-stage compaction hierarchy that rescues context windows to the bash classifier that prevents destructive commands -- reflects hard-won experience with real failure modes.

The audience for this book is engineers building long-running agent harnesses. Whether you are extending an existing system, evaluating an architecture for production deployment, or designing a new agent from scratch, cc provides a concrete reference point. This is not a tutorial on using Claude Code as an end user. It is a deep technical analysis of how cc works internally, why it works that way, and what the implications are for the discipline of harness engineering.

The book offers three reading paths. The linear path starts at Chapter 1 and proceeds through all fifty-seven chapters in order, building from foundations through subsystems to synthesis. The subsystem-focused path jumps to the Part that covers a specific subsystem -- Part III for the tool system, Part IV for multi-agent dispatch, Part VI for safety and permissions -- and reads outward from there. The pattern-focused path starts with Part X, which maps all twelve HER patterns and seventeen failure modes to concrete cc implementations, then follows cross-references backward into the chapters that provide the detailed evidence.

Each chapter follows a consistent structure: an Overview that states the chapter's thesis, a Source Map that lists the relevant source files, a Detailed Walkthrough that traces the code path with inline source citations, a Patterns and Failure Modes section that connects the implementation to HER's framework, a Diagrams section with Mermaid diagrams of the allowed types, and a Key Takeaways section. Source citations use the format `src/path/file.ts:Lnnn`. Diagrams use five allowed Mermaid types: flowchart, sequenceDiagram, stateDiagram-v2, classDiagram, and erDiagram.

The existence of this book is itself evidence of a shift. Five years ago, the idea of reading a production agent's source code to learn how to build better agents would have seemed academic. Today, with METR benchmarks showing that well-engineered harnesses can double task completion rates, and with the harness engineering discipline coalescing around shared patterns and failure taxonomies, it is essential. The code is the curriculum. This book is the guided tour.

## How to Read This Book

### Citation Format

All source citations follow the format `src/path/file.ts:Lnnn`, where `Lnnn` refers to the line number in the source tree at the repository SHA listed on the title page. When a citation references a directory rather than a specific file, the path ends with a trailing slash (e.g., `src/services/mcp/`).

### Diagram Conventions

This book uses five Mermaid diagram types, each serving a distinct purpose:

- **flowchart**: Control-flow and decision logic. Directed graphs showing branching paths and conditions.
- **sequenceDiagram**: Temporal interactions between components. Shows message passing, return values, and lifeline activation.
- **stateDiagram-v2**: State machines and lifecycle transitions. Shows valid states, transition triggers, and terminal states.
- **classDiagram**: Type hierarchies and interface contracts. Shows fields, methods, inheritance, and composition.
- **erDiagram**: Entity-relationship mappings. Shows how records, files, and data structures relate to each other.

### Chapter Structure

Every chapter contains six mandatory sections:

1. **Overview** -- The chapter's thesis and scope.
2. **Source Map** -- The source files covered, with line ranges.
3. **Detailed Walkthrough** -- The main body, tracing code paths with inline citations.
4. **Patterns and Failure Modes** -- Cross-references to HER patterns and failure modes.
5. **Diagrams** -- At least two Mermaid diagrams of the allowed types.
6. **Key Takeaways** -- Distilled findings and implications.

### Cross-Reference Notation

References to other chapters use the format "Chapter N" or "Part X". References to HER sections use the format "HER section N" or the section symbol (e.g., "section 5 Pattern 6"). When a chapter references a specific code snippet shown in another chapter, it includes the snippet checksum for verification.

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

- **Appendix A. Glossary of Terms**
- **Appendix B. Bibliography and Further Reading**
- **Appendix C. Cross-Reference Concordance**
