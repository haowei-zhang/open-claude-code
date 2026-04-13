# Consistency Audit — Chapter 55

## Term Conflicts
Scanning chapter for terminology registry conflicts:

1. "task" — Used consistently with canonical definition ("A durable unit of work tracked through status transitions"). PASS.
2. "subagent" — Used once ("sub-agent isolation" at line 26 in the classDiagram method name). The canonical term is "subagent" (no hyphen). The classDiagram uses "subagentIsolation()" which matches. But in the HER excerpt reference (L25-26), the term "sub-agent" appears in the HER spec itself, not the chapter's own voice. PASS (HER quote).
3. "compaction" — Used consistently with canonical definition. PASS.
4. "microcompact" — Used consistently ("cc's microcompact"). PASS.
5. "autocompact" — Used consistently with canonical definition. PASS.
6. "session" — Used consistently with canonical definition. PASS.
7. "permission mode" — Used consistently. PASS.
8. "hook" — Used consistently with canonical definition. PASS.
9. "memdir" — Used consistently with canonical definition. PASS.
10. "memory" — Used consistently with canonical definition. PASS.
11. "back-pressure" — Used consistently with canonical definition ("back-pressure" hyphenated). PASS.
12. "harness" — Used consistently with canonical definition. PASS.
13. "observation masking" — Used consistently with canonical definition. PASS.
14. "generator-evaluator" — Used consistently with canonical definition. PASS.
15. "circuit breaker" — Used consistently with canonical definition. PASS.
16. "coordinator mode" — Used consistently with canonical definition. PASS.
17. "checkpoint-restore" — Used consistently with canonical definition. PASS.
18. "cost metering" — Used consistently with canonical definition. PASS.
19. "feature gate" — Used consistently with canonical definition. PASS.
20. "high water mark" — Used consistently. PASS.

## Voice Drift
- The chapter is written in present tense, descriptive, cite-heavy style — consistent with house style.
- No voice drift detected.

## Proposed New Terms
1. "structured handoff" — The pattern of writing a progress file before clearing context and resuming from the file. Used extensively in this chapter as the escape hatch for compaction failure.
2. "compaction death spiral" — The self-reinforcing failure mode where each failed compaction attempt adds tokens, making the next attempt less likely to succeed.
3. "cost gate" — A mechanism that aborts or degrades a session when spend exceeds a configured threshold.
4. "evaluator ouroboros" — The degenerate case where LLM evaluation of LLM output creates a circular validation loop.
5. "back-pressure stack" — The tiered quality gate pipeline (Type System → Linter → Unit Tests → Integration Tests → E2E) where only failures surface to agent context.
