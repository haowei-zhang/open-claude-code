# Polish Audit: Chapter 26 — Memdir: Tiered Memory on Disk

## Verdict: REVISE

### Word Count

5449 words (within [5100, 7500] range).

### Forbidden Tokens

None found.

### Required Sections

All 6 mandatory sections present in correct order:
1. Overview — present
2. Data structures and contracts — present
3. Control flow — present
4. Edge cases and failure modes — present
5. Where cc diverges from the published pattern — present
6. Developer takeaways for building a long-running agent — present

### Snippet Counts

- Total: 27 (>= 4 required)
- In "Data structures and contracts" section: 6 (>= 1 required)
- In "Control flow" section: 10 (>= 1 required)
- No oversized snippets (all within 8-60 line range)
- No unexplained snippets

### Developer Takeaways

~220 words (within 150-300 word range). 8 numbered items covering: index caps, side queries, type taxonomy exclusions, path validation, staleness warnings, scan/retrieval separation, append-only logging, DIR_EXISTS_GUIDANCE.

### Style Issues

1. **Duplicate paragraph (line 147)**: The "RelevantMemory and the retrieval result" section including the type definition snippet appears twice — first at lines 110-122 and again at lines 147-157. The second occurrence is a verbatim copy of the first.

2. **Duplicate section (line 672)**: The "Memory and the 'already surfaced' deduplication" section appears twice — first at lines 660-664 and again at lines 672-676. The text is nearly identical in both instances.
