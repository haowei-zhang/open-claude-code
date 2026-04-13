# Cross-Reference Audit Report: Chapter 32

## Required HER References

| HER Ref | Found | Location |
|---------|-------|----------|
| §12 Guardrails | Yes | Overview, steps 1a-1g mapping to five-layer defense |
| §11 Human-in-the-Loop | Yes | Overview, tiered escalation, auto mode classifier |
| §6.13 Prompt Injection | Yes | Edge cases, safety checks, bypass-immune patterns |

## Divergence Section

The "Where cc diverges from the published pattern" section is 580 words and covers 6 substantive divergences:

1. Auto mode not in public API
2. Deny-first evaluation order
3. No rule priority between sources
4. Safety checks bypass-immune even for hooks
5. The passthrough behavior
6. Iron gate is a feature flag, not compile-time

Verdict: **pass** (all required refs present, divergence section substantive)
