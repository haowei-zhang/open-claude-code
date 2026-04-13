# Cross-Reference Audit: Chapter 15 — Search, LSP, and Code Analysis Tools

## Required HER refs

The brief requires:
1. §5 Pattern 9 Progressive Tool Expansion
2. §6.8 Tool Explosion

## Verification

### §5 Pattern 9 Progressive Tool Expansion

**FOUND.** The chapter references Pattern 9 multiple times:
- Overview: "Together, these tools implement HER Pattern 9 (Progressive Tool Expansion)"
- Control flow section: "The ToolSearchTool is the linchpin of cc's progressive tool expansion strategy"
- Where cc diverges: "HER Pattern 9 (Progressive Tool Expansion) recommends starting with fewer than 20 tools and activating more on demand"

The chapter engages substantively with this pattern, explaining how cc implements it through the `shouldDefer` flag and `ToolSearchTool`.

### §6.8 Tool Explosion

**FOUND.** The chapter references failure mode 6.8:
- Overview: "address HER failure mode 6.8 (Tool Explosion)"
- Where cc diverges: "HER failure mode 6.8 (Tool Explosion) warns that too many tools degrade selection accuracy"

The chapter explains how cc addresses this through deferral and notes the limitation of lacking tool eviction.

## Divergence section

The "Where cc diverges from the published pattern" section is substantive at approximately 250+ words. It identifies three concrete divergences:
1. No tool-eviction mechanism
2. Description fetching is lazy and expensive
3. Search is keyword-based, not semantic

Each divergence includes a specific remediation suggestion. This is a strong divergence section.

## Distinct HER references

At least 2 distinct HER references are present: Pattern 9 and failure mode 6.8. PASS.

## Issues

No missing required refs. Divergence section is substantive. All required HER refs are present and correctly cited.

## Summary

- Required refs found: 2/2
- Divergence section: ~250 words (well above 150-word threshold)
- No bad section references
