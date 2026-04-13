# Crossref Audit: Chapter 26 — Memdir: Tiered Memory on Disk

## Verdict: REVISE

### Required HER References

| Required | Found? |
|----------|--------|
| §5 Pattern 3 Tiered Memory | Yes — explicitly cited in Overview ("HER Pattern 3 (Tiered Memory)") |
| §7.1 Persistent Instruction File | No — not explicitly cited by section number |

The chapter references the ETH Zurich study from §7.1 in the "Closed type taxonomy" divergence section, attributing the finding that "verbose context files reduce task success rates by over 20%" to "arXiv:2602.11988". However, it does not cite "§7.1 Persistent Instruction File" as the HER reference. The brief requires explicit citation of §7.1.

### Divergence Section

The "Where cc diverges from the published pattern" section is substantive with 3 subsections:
1. Side-query relevance selection vs. embedding-based retrieval
2. Closed type taxonomy vs. free-form notes
3. Path-level write carve-out for memory files

Word count: ~420 words (well above the 150-word minimum).

### Summary

- Required refs found: 1/2
- Missing: §7.1 Persistent Instruction File (implicit but not explicitly cited)
- Divergence section: substantive, 420 words
