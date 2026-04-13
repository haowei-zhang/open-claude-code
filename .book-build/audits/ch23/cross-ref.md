# Cross-Reference Audit: Chapter 23 — Teammates and In-Process Collaboration

## Required HER References

1. **§9.1 in-process tier** — Found. The Overview section (line 7) explicitly references "HER's three tiers of multi-agent orchestration (Section 9.1)" and describes the in-process tier as "same process, isolated context windows." The Data structures section further discusses the in-process tier's architectural significance.

2. **§11.5 Handoff Protocols** — Found. The "Structured message types" section (line 70) states: "These structured messages implement HER's handoff protocol (Section 11.5)." The shutdown and plan-approval protocols are discussed as implementations of handoff patterns. The "Where cc diverges from the published pattern" section explicitly discusses HER Section 11.5's handoff document and checkpoint-review protocols.

## Divergence Section

The "Where cc diverges from the published pattern" section (lines 295-338) covers three substantive divergences:

1. **No checkpoint-review protocol** — HER Section 11.5 prescribes mandatory human review at 25% and 75%. cc's plan-approval is optional.
2. **File-based communication not default** — HER Section 9.3 recommends file-based communication. cc uses in-memory queue and write-only-log mailbox.
3. **Missing handoff document** — HER Section 11.5 prescribes structured handoff documents. cc has ad-hoc messages only.

Word count of divergence section: approximately 220 words (substantive, not a one-liner).

## Summary

- All 2 required HER refs are present and correctly cited.
- Divergence section is substantive (220+ words).
- 2 distinct HER references present (§9.1 and §11.5).
- No wrong section references.
