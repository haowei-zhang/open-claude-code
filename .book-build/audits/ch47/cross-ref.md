# Cross-Reference Audit Report — Chapter 47

## Summary

Verdict: **pass**

## Required HER References

| Ref | Found | Notes |
|-----|-------|-------|
| §10.2 Task Sizing Research (METR) | Yes | Cited in Overview (line 7) and divergence section (line 507) |
| §10.1 One-Task-Per-Session | Yes | Cited in Overview (line 7) and divergence section (line 509) |

## Divergence Section

The "Where cc diverges from the published pattern" section is substantive at ~287 words. It covers three specific divergences:

1. The cron system does not enforce the METR 15-30 minute task sizing bound — the discipline is left to the prompt.
2. The 50-job maximum could theoretically violate one-task-per-session, though the idle-REPL check prevents this in practice.
3. The /loop skill's autonomous self-scheduling is a novel use not anticipated by HER.

All three are well-supported and substantive. No issues found.
