# Cross-Reference Audit: Chapter 4

## Required HER References
The chapter brief requires:
- §4 Fowler taxonomy
- §12 supply chain

## Verification

### §4 Fowler taxonomy
FOUND. The chapter extensively cites section 4:
- "The HER thesis (section 4) presents Fowler's taxonomy of guides and sensors"
- "Fowler's taxonomy focuses on the harness as a consumer of language and runtime features"
- "Fowler's concept of harnessability (HER section 4.5) is directly relevant"
- "Fowler's caveat is important"
- Multiple references to computational guides, inferential controls, and harnessability

### §12 supply chain
FOUND. The chapter cites section 12:
- "The HER section 12 (supply chain) raises another concern"
- "cc's dependency on Bun means that the supply chain includes the Bun runtime itself"
- "The HER recommends vetting all supply chain dependencies"
- Supply chain risk discussed in takeaway #8

## Divergence Section
The "Where cc diverges from the published pattern" section is present and substantive at 484 words. It discusses:
1. How cc diverges from Fowler's consumer model - the harness is also a *producer* of runtime features
2. The co-design cost: portability constraints
3. Supply chain risk from tight runtime coupling
4. TypeScript's type system as a computational guide
5. React component model as scaffold for guides and sensors
6. Harnessability factors and technical debt concerns

## Issues
- No missing required references
- Divergence section is substantive (484 words, well above 150-word minimum)
- Both required HER refs are present

## Summary
- Required refs found: 2/2
- Divergence section: 484 words
- No issues found
