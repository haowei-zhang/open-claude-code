# Bibliography

The following works are referenced throughout this book. Entries are grouped by category and numbered to align, where possible, with the source numbering in the Harness Engineering Report (HER, Section 22). Each entry provides the author, title, URL where available, and year of publication or last update.

---

## Canonical Sources

1. Anthropic. "Harness Design for Long-Running Apps." Prithvi Rajasekaran. https://www.anthropic.com/engineering/harness-design-long-running-apps (March 24, 2026). One of four foundational sources for the HER; addresses session management, context lifecycle, and reliability patterns for agents that run beyond a single turn.

2. Anthropic. "Effective Harnesses for Long-Running Agents." Justin Young et al. https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents (November 2025). The earliest canonical source; defines the ORIENT-SETUP-VERIFY-SELECT-IMPLEMENT-TEST-UPDATE-EXIT session protocol and the one-task-per-session rule.

3. OpenAI. "Harness Engineering with Codex." https://openai.com/index/harness-engineering/ (February 11, 2026). Published six days after Hashimoto's blog post, lending institutional weight to the term. Covers the Codex agent's approach to tool definition, permission scoping, and execution environments.

4. Huntley, Geoffrey. "Ralph Wiggum as a Software Engineer." https://ghuntley.com/ralph/ (July 14, 2025). The simplest possible harness: a bash loop around an LLM. Reported a $297 contract completed by a $50k human equivalent. Cited throughout the HER as a lower-bound baseline for harness complexity.

---

## Taxonomy and Theory

5. Fowler, Martin. "Harness Engineering." https://martinfowler.com/articles/harness-engineering.html (last modified April 2, 2026). The most systematic taxonomy to date. Introduces Guides (feedforward) vs. Sensors (feedback), Computational vs. Inferential controls, and the concept of "harnessability." Applies Ashby's Law to agent regulation.

6. Cen Runzhe. "From Prompt Engineering to Harness Engineering: The Layer That Makes AI Agents Actually Work." https://medium.com/@cenrunzhe/from-prompt-engineering-to-harness-engineering-the-layer-that-makes-ai-agents-actually-work-466fe0489fbe (2026). Traces the evolution from prompt engineering through context engineering to harness engineering; attributes the concept's framing to OpenAI.

7. LangChain. "The Anatomy of an Agent Harness." https://blog.langchain.com/the-anatomy-of-an-agent-harness/ (2025). Introduces the equation Agent = Model + Harness that the HER adopts as its core definition. Decomposes the harness into execution environment, tool definitions, control loops, guardrails, and observability.

8. LangChain. "State of Agent Engineering." https://www.langchain.com/state-of-agent-engineering (2025). Survey of 1,340 respondents conducted November 18 through December 2, 2025. Provides quantitative data on adoption rates, observability practices, and multi-agent deployment patterns. Cited for the finding that 89% of organizations with production agents have implemented observability.

---

## Practitioner Perspectives

9. Hashimoto, Mitchell. "My AI Adoption Journey." https://mitchellh.com/writing/my-ai-adoption-journey (February 5, 2026). Co-founder of HashiCorp. The most-cited popularizer of the term "harness engineering." Articulates the principle: "Anytime you find an agent makes a mistake, you take the time to engineer a solution such that the agent never makes that mistake again."

10. Raza, Muhammad. "Harness Engineering -- The DevOps Skill Nobody Told You About." https://muhammadraza.me/2026/harness-engineering-devops-perspective/ (2026). Argues that harness engineering maps directly to existing DevOps practices: execution environments = CI runners, tool definitions = API design, control loops = health checks, guardrails = IAM policies. Cited in the HER's limitation analysis (Section 18) as evidence that harness engineering is substantially DevOps applied to a probabilistic workload.

11. Willison, Simon. "Agentic Engineering Patterns." https://simonwillison.net/guides/agentic-engineering-patterns/ (from February 23, 2026). A curated collection of patterns for building reliable agent systems, with emphasis on deterministic verification over LLM-based evaluation.

12. Willison, Simon. "Red/Green TDD for Agents." https://simonwillison.net/guides/agentic-engineering-patterns/red-green-tdd/ (2026). Proposes writing failing tests first, confirming they fail, then letting the agent make them pass. Offers a more disciplined alternative to LLM-based evaluation by anchoring quality in deterministic outcomes.

13. HumanLayer. "Skill Issue -- Harness Engineering for Coding Agents." https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents (2026). Attributes early usage of "harness engineering" to a contributor identified as "Viv." Covers human-in-the-loop design, tool permission scoping, and the role of CLAUDE.md files.

14. Generative Programmer. "12 Agentic Harness Patterns from Claude Code." https://generativeprogrammer.com/p/12-agentic-harness-patterns-from (2026). Reverse-engineers Claude Code's architecture into twelve named patterns, including Persistent Instruction File, Scoped Context Assembly, Tiered Memory, Dream Consolidation, Progressive Context Compaction, Explore-Plan-Act Loop, Context-Isolated Subagents, Fork-Join Parallelism, Progressive Tool Expansion, Command Risk Classification, Single-Purpose Tool Design, and Deterministic Lifecycle Hooks. The HER adopts this taxonomy wholesale as Section 5.

15. celesteanders. "Building a Harness with Claude." https://www.reddit.com/r/ClaudeAI/comments/1s9jm0d/ (2025). Reddit post and discussion on practical harness construction. Favors JSON-based task lists with boolean pass/fail status and append-only constraints over Markdown approaches.

16. celesteanders. "harness: Best Practices." https://github.com/celesteanders/harness/blob/main/docs/best-practices.md (2025). Open-source reference implementation of a Claude harness with structured task tracking and session protocols.

17. HumanLayer. "Writing a Good CLAUDE.md." https://www.humanlayer.dev/blog/writing-a-good-claude-md (2026). Guidelines for the persistent instruction file format, including the recommended three-part structure (WHY / WHAT / HOW) and the 60-line optimal length recommendation.

18. Builder.io. "How to Write a Good CLAUDE.md." https://www.builder.io/blog/claude-md-guide (2026). Complementary guide to CLAUDE.md authorship with emphasis on project-specific conventions and minimal requirements over verbose documentation.

---

## Academic Papers

19. Natural-Language Agent Harnesses (NLAHs). https://arxiv.org/html/2603.25723v1 (March 2026). Externalizes harness control logic as portable, editable natural-language artifacts. Key ablation results: self-evolution module +4.8% on SWE-bench; file-backed state +1.6% on SWE-bench and +5.5% on OSWorld; verifier -0.8% on SWE-bench; multi-candidate search -2.4%. Demonstrates that more structure does not automatically improve performance.

20. Meta-Harness: End-to-End Optimization. https://arxiv.org/abs/2603.28052 (March 2026). An outer-loop system that searches over harness code using an agentic proposer with filesystem access to all prior candidates' source code, scores, and execution traces. Achieves +7.7 points over state-of-the-art context management with 4x fewer tokens and +4.7 points on 200 IMO-level problems. Suggests that harness configurations can be systematically optimized rather than hand-tuned.

21. AutoHarness: Automatically Synthesizing Code Harnesses. https://arxiv.org/abs/2603.03329 (February 2026). LLMs automatically generate constraint-enforcement harnesses through iterative refinement with environment feedback. Key result: a smaller model (Gemini-2.5-Flash) with a synthesized harness outperformed a larger model (Gemini-2.5-Pro) without one. The harness prevents illegal moves proactively rather than eliminating them retroactively.

22. Building AI Coding Agents for the Terminal (OPENDEV). https://arxiv.org/html/2603.05344v1 (March 2026). Describes the five-layer defense-in-depth model adopted by the HER: prompt-level guardrails, schema-level restrictions, runtime approval, tool-level validation, and lifecycle hooks. Directly informs the HER's security threat model.

23. "Towards a Science of AI Agent Reliability." https://arxiv.org/html/2602.16666v1 (2026). Proposes a scientific framework for evaluating and improving agent reliability, including formal definitions of failure modes and recovery strategies.

24. "Context Window Overflow." https://arxiv.org/html/2511.22729v1 (2025). Analyzes the degradation of model performance as context windows fill, providing empirical evidence for the "context rot" failure mode and the mid-window attention degradation phenomenon.

25. ACRFence: Preventing Semantic Rollback Attacks in Agent Checkpoint-Restore. https://arxiv.org/abs/2603.20625 (March 2026). Identifies a novel threat model: agents re-synthesize subtly different requests after restore, causing duplicate external side effects. Two attack classes defined: Action Replay and Authority Resurrection. Proposes replay-or-fork semantics and idempotency keys. Directly relevant to cc's session persistence and Dream Task subsystem.

26. "Evaluating AGENTS.md." ETH Zurich. https://arxiv.org/abs/2602.11988 (2026). One of the few empirical evaluations of persistent instruction files. Finds that context files tend to reduce task success rates compared to providing no repository context, while also increasing inference cost by over 20%. Applies to both LLM-generated and developer-committed context files. Recommends describing only "minimal requirements." Cited extensively in the discussion of CLAUDE.md design.

27. METR (Model Evaluation and Threat Research). "Measuring AI Ability to Complete Long Tasks." https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/ (March 2025). Key findings: AI task duration doubles approximately every 7 months; 95% per-step reliability yields only 36% over 20 steps; doubling task duration quadruples failure rate. Updated Time Horizon 1.1 model (January 2026) suggests the rate has accelerated to approximately 4.3 months.

28. Ord, Toby. "Is there a Half-Life for the Success Rates of AI Agents?" https://arxiv.org/abs/2505.05115 (May 2025). Proposes that AI agent failure follows a constant hazard rate analogous to radioactive decay. Finds that Claude 3.7 Sonnet achieves 50% success on tasks up to 59 minutes but requires tasks under 15 minutes for 80% success. Every AI agent experienced degraded success rates as task duration increased.

---

## Platform Documentation

29. Cursor. "Agent Best Practices." https://cursor.com/blog/agent-best-practices (2025). Documentation of Cursor's approach to agent interaction design, including Composer 2's background agents and IDE-integrated permission UI.

30. Cursor. "Composer 2." https://cursor.com/blog/composer-2 (2025). Describes Cursor's background agent system and model-aware tuning per model variant.

31. Cognition. "Devin: 2025 Performance Review." https://cognition.ai/blog/devin-annual-performance-review-2025 (2025). Performance benchmarks for the Devin autonomous coding agent, including PR-based verification and sandboxed execution.

32. Anthropic. "Claude Code: Best Practices." https://code.claude.com/docs/en/best-practices (2026). Official Anthropic documentation on recommended usage patterns for Claude Code, covering CLAUDE.md, permission modes, and session design.

33. Anthropic. "Claude Code: Hooks Guide." https://code.claude.com/docs/en/hooks-guide (2026). Official documentation for the lifecycle hook system, including event types, hook configuration, and exit code semantics.

---

## Industry Analysis

34. Osmani, Addy. "The Code Agent Orchestra." https://addyosmani.com/blog/code-agent-orchestra/ (2026). Surveys the emerging landscape of multi-agent coding systems and their orchestration patterns.

35. Osmani, Addy. "Self-Improving Coding Agents." https://addyosmani.com/blog/self-improving-agents/ (2026). Examines agent systems that iteratively improve their own harness configurations and code quality.

36. O'Reilly. "Conductors to Orchestrators: The Future of Agentic Coding." https://www.oreilly.com/radar/conductors-to-orchestrators-the-future-of-agentic-coding/ (2026). Positions the evolution from human conductors to AI orchestrators in the software development lifecycle.

37. InfoQ. "OpenAI Harness Engineering." https://www.infoq.com/news/2026/02/openai-harness-engineering-codex/ (February 2026). Industry analysis of OpenAI's harness engineering announcement and its implications for the emerging discipline.

38. Data Science Dojo. "Harness Engineering." https://datasciencedojo.com/blog/harness-engineering/ (2026). Introductory overview of harness engineering concepts for a data science audience.

---

## Context and Memory

39. JetBrains Research. "Efficient Context Management." https://blog.jetbrains.com/research/2025/12/efficient-context-management/ (December 2025). Provides the verified evidence for observation masking: 52% cost reduction by hiding irrelevant tool outputs and only surfacing errors. Cited as the single most effective cost optimization with empirical support.

40. Redis. "Context Window Overflow 2026." https://redis.io/blog/context-window-overflow/ (2026). Analysis of context window overflow patterns and mitigation strategies using external memory stores.

41. Maxim.ai. "Context Window Management Strategies for Long-Context AI Agents and Chatbots." https://www.getmaxim.ai/articles/context-window-management-strategies-for-long-context-ai-agents-and-chatbots/ (2026). Practical strategies for managing context window utilization in production agent systems.

42. Anthropic. "Effective Context Engineering for AI Agents." https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents (2026). Anthropic's guidance on context engineering practices, including prompt construction, information retrieval, and context window optimization.

---

## Reliability and Safety

43. Guardrails.md Protocol. https://guardrails.md/ (2026). Defines a Sign structure (Trigger, Instruction, Reason, Provenance) for declarative guardrail specification in agent systems.

44. NeMo Guardrails. https://guardrailsai.com/ (2025). NVIDIA's open-source framework for building guardrails around LLM applications, including input/output validation and topic control.

45. UiPath. "10 Best Practices for Reliable Agents." https://www.uipath.com/blog/ai/agent-builder-best-practices (2025). Enterprise-focused recommendations for agent reliability in production RPA contexts.

46. Maxim.ai. "Ensuring AI Agent Reliability in Production." https://www.getmaxim.ai/articles/ensuring-ai-agent-reliability-in-production/ (2026). Operational guidance for maintaining agent reliability at scale.

47. Anthropic. "Demystifying Evals for AI Agents." https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents (2026). Anthropic's framework for evaluating agent performance, including metric selection and evaluation design.

---

## Frameworks and Tools

48. celesteanders/harness. https://github.com/celesteanders/harness (2025). Open-source Claude harness implementation with structured JSON task tracking, append-only status updates, and session protocol enforcement.

49. snarktank/ralph. https://github.com/snarktank/ralph (2025). Open-source implementation of the Ralph bash-loop pattern from Geoffrey Huntley's blog. The simplest harness: a shell loop around an LLM API call.

50. eyaltoledano/claude-task-master. https://github.com/eyaltoledano/claude-task-master (2025). A task management system for Claude Code sessions with structured task decomposition and progress tracking.

51. ai-boost/awesome-harness-engineering. https://github.com/ai-boost/awesome-harness-engineering (2026). Curated list of harness engineering resources, tools, and references.

52. Chachamaru127/claude-code-harness. https://github.com/Chachamaru127/claude-code-harness (2026). A community-built harness configuration for Claude Code with custom tool definitions and permission rules.

---

## Additional Sources

53. HAMY. "AI Checkpointing." https://hamy.xyz/blog/2025-07_ai-checkpointing (July 2025). Explores the challenge of checkpointing and restoring AI agent sessions, including the problem of irreversible side effects.

54. Zylos. "Long-Running AI Agents." https://zylos.ai/research/2026-01-16-long-running-ai-agents (January 16, 2026). Research on the operational challenges of agents that run for hours or days, including session management and state persistence.

55. Zylos. "AI Agent Context Compression Strategies." https://zylos.ai/research/2026-02-28-ai-agent-context-compression-strategies (February 28, 2026). Comparison of context compression approaches for long-running agent sessions.

56. Towards Data Science. "Why Your Multi-Agent System Is Failing: Escaping the 17x Error Trap of the Bag of Agents." https://towardsdatascience.com/why-your-multi-agent-system-is-failing-escaping-the-17x-error-trap-of-the-bag-of-agents/ (2026). Analysis of compounding error rates in multi-agent architectures.

57. Schmid, Philipp. "Agent Harness 2026." https://www.philschmid.de/agent-harness-2026 (2026). Practitioner perspective on harness engineering from the Hugging Face ecosystem.

58. Bouchard, Louis. "Harness Engineering." https://www.louisbouchard.ai/harness-engineering/ (2026). Accessible introduction to harness engineering concepts for a general technical audience.

59. Firecrawl. "What Is an Agent Harness." https://www.firecrawl.dev/blog/what-is-an-agent-harness (2026). Defines the agent harness concept with emphasis on web-scraping and data-extraction tool integration.

60. Agent Engineering. "Harness Engineering in 2026: The Discipline That Makes AI Agents Production-Ready." https://www.agent-engineering.dev/article/harness-engineering-in-2026-the-discipline-that-makes-ai-agents-production-ready (2026). Overview article framing harness engineering as a distinct discipline with its own practices and tooling.

61. AWS. "Build Durable AI Agents with LangGraph and Amazon DynamoDB." https://aws.amazon.com/blogs/database/build-durable-ai-agents-with-langgraph-and-amazon-dynamodb/ (2026). Enterprise-grade reference architecture for persistent agent state using LangGraph orchestration and DynamoDB-backed event sourcing. Cited in the HER's decision framework for enterprise distributed state management.

---

## cc Internal References

62. Anthropic. Claude Code source code (leaked via npm sourcemap, March 31, 2026). Discovered by Chaofan Shou (@Fried_rice). The primary subject of this book. Repository mirror archived for research purposes. The codebase comprises a 785KB `main.tsx` entry point, 40+ tools, a custom React/Ink terminal renderer, multi-agent orchestration via the coordinator module, IDE bridge integration, the Buddy Tamagotchi system, the autoDream consolidation service, and the KAIROS proactive assistant. Referenced throughout this book via file paths in the `src/` directory.

63. TerminalBench 2.0 Leaderboard. https://tbench.ai (as of April 2026). Benchmark demonstrating the 24.9 percentage-point spread for Claude Opus 4.6 across different harnesses. Provides empirical evidence that harness design matters more than model selection for production reliability.

64. Anthropic. Claude API Documentation. https://docs.anthropic.com/ (2026). Official API reference for the Anthropic SDK, including streaming, prompt caching, tool use, and model specifications for the Opus, Sonnet, and Haiku model tiers.

65. Anthropic. Claude Code Official Documentation. https://code.claude.com/docs/ (2026). Official documentation for Claude Code, covering installation, configuration, CLAUDE.md, hooks, skills, MCP server integration, permission modes, and the query loop.

66. Bun Runtime. https://bun.sh/ (2026). The JavaScript/TypeScript runtime used by Claude Code. Notable for fast startup times, native TypeScript support, and built-in bundling that contributed to the sourcemap leak incident.

67. Ink (React for CLI). https://github.com/vadimdemedes/ink (2026). The React-based terminal rendering framework used by Claude Code for its interactive UI. Provides Yoga-based layout, component model, and focus management for terminal applications.

68. Model Context Protocol (MCP). https://modelcontextprotocol.io/ (2025-2026). The open protocol for connecting AI models to external tools and data sources. Claude Code implements MCP client support with stdio and SSE transports, OAuth authentication, and deferred tool loading.
