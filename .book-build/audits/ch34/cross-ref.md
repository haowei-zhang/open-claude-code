# Cross-Reference Audit: Chapter 34 - Filesystem Permissions and Path Guards

## Required HER References

The brief requires these HER references:
1. §12.4 Data Exfiltration
2. §6.15 Data Leakage Between Contexts

## Verification

### §12.4 Data Exfiltration
**Found**: Yes. The Overview section (line 7) explicitly references "HER's threat model for data exfiltration (Section 12.4)" and discusses supply chain attacks via configuration file writes. The "Where cc diverges" section (line 391) also references "HER Section 12.4 recommends allowlisting external endpoints."

### §6.15 Data Leakage Between Contexts
**Found**: Yes. The Overview section (line 7) explicitly references "data leakage between contexts (Section 6.15)". The "Where cc diverges" section (line 393) also references "HER Section 6.15 identifies file-based communication as a source of data leakage."

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive at approximately 280 words. It covers:
1. How cc proactively blocks dangerous paths vs. HER's recommendation for pattern monitoring
2. How cc's internal path carve-outs mitigate data leakage
3. How denial tracking diverges from HER's tiered escalation model (Section 11.2)

## Issues

None. Both required HER references are cited, the divergence section is substantive (>150 words), and the references are accurate.
