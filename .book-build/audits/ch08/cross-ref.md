# Cross-Reference Audit: Chapter 8 — Talking to Anthropic: Streaming

## Required HER References

The brief requires these HER references:
1. §13 cost management (observation masking)
2. §6.14 Model regression from provider updates

## Verification

### §13 cost management (observation masking)
- **Status**: FOUND. The chapter references HER section 13 in the "Where cc diverges from the published pattern" section (lines 269-279). It discusses per-session cost tracking (`totalCostUSD`), token budget enforcement, and explicitly notes cc's divergence from HER's per-task cost caps, dead-letter queue, and per-hour spend rate alerts. The chapter also references observation masking (52% cost reduction) and connects it to the microcompact system.
- **Verdict**: CITED and substantive.

### §6.14 Model regression from provider updates
- **Status**: FOUND. The chapter has a dedicated subsection "Model regression from provider updates" (lines 213-226). It discusses explicit model versioning, beta header gating, migration system, fallback model selection, and prompt cache break detection as mitigations.
- **Verdict**: CITED and substantive.

## Divergence Section

The "Where cc diverges from the published pattern" section spans lines 269-279, approximately 220 words. It explicitly covers:
- Per-session cost tracking (implemented)
- Token budget enforcement (implemented)
- No per-task cost cap (divergence)
- No dead-letter queue (divergence)
- No per-hour spend rate alert (divergence)
- Observation masking via microcompact (partial implementation)
- No per-outcome tracking per HER 13.4 (divergence)

**Divergence section word count**: ~220 words (above 150-word minimum).
**Substantive**: Yes — specific file references, specific feature gaps identified.

## Issues

- None. Both required HER refs are present and substantive. The divergence section is well-developed.
