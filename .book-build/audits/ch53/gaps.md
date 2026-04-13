# Gaps Audit: Chapter 53

## Overview vs Synopsis

The brief synopsis states: "One section per pattern. For each: the pattern (HER), cc's implementation (with file paths), divergences, gaps, lessons."

All 12 patterns have dedicated Control Flow subsections. The Overview section frames the chapter correctly.

## Source Files

Chapter 53 is a meta-chapter with `source_files: []` in the manifest. No uncited brief files.

## Mandated Diagrams

- (a) erDiagram mapping 12 patterns to cc subsystems: PRESENT
- (b) flowchart showing pattern interactions in a running query: PRESENT

## Minimum Counts

- Citations: 34 (minimum: 6) - PASS
- Diagrams: 2 (minimum: 2) - PASS
- Snippets: 14 (minimum: 4) - PASS

## Missing Divergences

The "Where cc diverges from the published pattern" section covers 8 of 12 patterns. Missing divergences:

1. **Pattern 3 (Tiered Memory)**: No explicit divergence. Possible divergence: HER describes a strict three-tier model, but cc's memory system has additional nuance (the `alreadySurfaced` parameter preventing re-selection, the `recentTools` parameter filtering active-tool docs, the staleness warning system).

2. **Pattern 5 (Progressive Context Compaction)**: No explicit divergence. Possible divergence: HER describes four compaction layers as sequential, but cc's microcompact can be triggered independently of the hierarchy via cached microcompact, and the `COMPACTABLE_TOOLS` set means not all tool results are equally compactable.

3. **Pattern 8 (Fork-Join Parallelism)**: No explicit divergence. Possible divergence: HER describes fork-join as a multi-agent pattern, but cc's fork path is a single-child delegation where the parent continues executing (not a true fork-join where the parent waits). The join is implicit (the child sends results back) rather than structured.

4. **Pattern 10 (Command Risk Classification)**: No explicit divergence. Possible divergence: HER describes deterministic pre-parsing, but cc's external-build stub means classification is effectively disabled for external users, making the "deterministic" guarantee Anthropic-internal only.

Note: Patterns 3, 5, 8, and 10 ARE covered in the Edge Cases section, but lack explicit divergence analysis.

## Verdict: revise

Four uncovered divergence topics. All other checks pass.
