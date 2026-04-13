# Chapter 51 Rewrite Prompt

You are writing Chapter 51 of a 57-chapter technical book titled "Deep Research and Development Guide of CC Source Code". You are a single-use subagent. Your only job is to write this one chapter, then exit with a status line.

# Chapter brief
Title: Debug Logs, Diagnostics, and Doctor
Slug: debug-logs-diagnostics-doctor
Target word count: 4500 (acceptable range: 3825–5625)
Output file: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch51-debug-logs-diagnostics-doctor.md

# Synopsis
Debug logging, diag logs, doctor screen, security review command, perf issue collection.

# Source files you MUST read and cite
- `src/utils/debug.ts` (268 LOC) — Read the full file.

Read each listed source file. You are forbidden from citing files not on this list.

# HER (Harness Engineering Report) cross-references you MUST integrate
Read the pre-extracted excerpts at: /home/hwzhang/build/open-claude-code/.book-build/her-excerpts/ch51.md
Cross-reference these exact HER sections:
- §14.2 Observability Gap
- §6.6 Silent Failures

# Diagram requirements (at minimum)
(a) classDiagram of logging layers
(b) sequenceDiagram of a doctor run
Only use: flowchart, sequenceDiagram, stateDiagram-v2, classDiagram, erDiagram. Each mermaid block goes in a fenced ```mermaid code block. No other diagram types.

# Terminology registry (authoritative)
Read /home/hwzhang/build/open-claude-code/.book-build/terminology.json and use the terms with the exact definitions given. If you need a new term, pick one that doesn't conflict; a consistency auditor will reconcile.

# House style (non-negotiable)
- Write the chapter body as Markdown. Use `##` for top-level headings inside the chapter (the chapter title itself is an `#` heading on the first line).
- Cite every factual claim with `src/path/to/file.ts:Lnnn` (line-number-exact). For ranges use `src/path/to/file.ts:Lnnn-Lmmm`. Citations go inline in backticks.
- You MUST have at least 6 unique citations, at least 2 mermaid diagrams, and **at least 4 quoted code snippets** from the listed source files.
- You MUST include these sections in this exact order after the chapter title:
  1. ## Overview
  2. ## Data structures and contracts (MUST include at least 1 quoted code snippet showing a type, schema, or interface definition from the codebase)
  3. ## Control flow (MUST include at least 1 mermaid diagram AND at least 1 quoted code snippet showing a key function body)
  4. ## Edge cases and failure modes
  5. ## Where cc diverges from the published pattern
  6. ## Developer takeaways for building a long-running agent
- **Quoted code snippet format** (strict):
  - Use a fenced code block tagged with the correct language (`typescript`, `tsx`, `javascript`, `json`, `bash`, `yaml`).
  - The FIRST line inside the fence must be a comment with the exact source path and line range plus a short caption. Examples:
    `// src/utils/debug.ts:L19-L25 DebugLogLevel type and level ordering`

# PREVIOUS ATTEMPT FAILED — Address these audit failures

This is a REWRITE. The previous attempt failed the following audit checks. You MUST address each one:

1. **polish: REWRITE** — Word count was 3338, below the minimum of 3825. You must reach at least 3825 words. Ensure substantial coverage of all topics.

2. **gaps: REWRITE** — These topics from the synopsis were NOT covered at all:
   - doctor screen
   - security review command
   - perf issue collection
   - diag logs
   These are MANDATORY topics that MUST appear in the chapter. Even though they may not have explicit source files, they are part of the diagnostic subsystem and must be discussed with references to how cc handles them (even if only at a conceptual/architectural level).

3. **accuracy: REVISE** — Snippet drift issues:
   - Line 28: Snippet caption says L19-L25 but includes type export from L18. Fix caption to L18-L26.
   - Line 45: Whitespace drift in BufferedWriter configuration snippet. Match source exactly.
   - Line 107: Content drift in logForDebugging snippet - blank line handling differs. Match source exactly.
   - Line 138: Whitespace drift in isDebugMode snippet. Match source exactly.
   ALL code snippets MUST be verbatim from the source file. Read the source file and copy snippets character-for-character.

4. **consistency: REVISE** — Term conflict: "subagent" was used as "sub-agent". The terminology registry defines the term as "subagent" (no hyphen). Use "subagent" consistently.

5. **diagrams: REVISE** — Missing required diagrams:
   - classDiagram of logging layers (NOT FOUND — must include)
   - sequenceDiagram of a doctor run (NOT FOUND — must include)
   Both required diagrams must be present and properly formed.

6. **cross-ref: PASS** — No issues.

# Universal rules (apply to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after. The parent receives this line as your sole output and any extra bytes bloat its context.
2. **Do all your work via file writes and intermediate tool calls.** If you want to explain what you did, write it to a file under `.book-build/` (e.g., the audit prose reports go to `.md` files, chapter bodies go to chapter files). Do NOT put explanations in your final response.
