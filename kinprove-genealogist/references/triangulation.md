# Triangulation

A **triangulation** is three (or more) people who share the same DNA segment on the same chromosome at the same coordinates. When valid, it strongly implies they share a common ancestor at that segment's locus.

## The trio requirement

Two-person matches are not triangulations — they're just IBD (identity by descent) pair matches. For a true triangulation you need:

1. **Three people**, all pair-matching one another on the same chromosome
2. **Overlapping segment coordinates** (typically ≥7 cM overlap)
3. **All three matching each other**, not just A↔B and A↔C separately

Two-of-three pair matches without the third pair = "ICW" (in-common-with), NOT triangulation.

## Why it matters

A pair match could be:

- A real shared segment from a common ancestor (IBD)
- A coincidental match from different ancestors (IBC — identity by chance)
- A pile-up region (frequent matches in many people, low information)

Triangulation reduces the IBC probability geometrically — three people coincidentally matching at the same locus is rare. So a triangulated segment is high-confidence evidence of a common ancestor.

## Kinprove triangulation pipeline

Kinprove computes triangulations in a Phase 2 batch job over C(N,3) trios from Phase 1 pairwise IBD. Result: `DnaTriangulatedSegment` rows.

When using the Kinprove MCP connector, prefer triangulated segments over raw pairwise matches when building hypotheses. Tool: `get_kit_triangulations` (Claude surfaces it as `mcp__kinprove__get_kit_triangulations`) returns the triangulated segments touching a specific DNA kit.

## X-chromosome triangulation

X-segments form their own triangulations (`chromosome='X'`). X-triangulation is **additive evidence only** in Kinprove's hypothesis scoring — it boosts confidence but never penalizes (because X-DNA inheritance is sparse and unidirectional from father→daughter only).

## Limitations

- Triangulation evidence still doesn't TELL you which ancestor — you need tree work to identify the MRCA.
- Endogamy creates pseudo-triangulations: people sharing many ancestors triangulate "everywhere" without a unique MRCA. Use endogamy mode + tighter cutoffs.
- Pile-up regions (HLA, centromeres) generate spurious triangulations. Kinprove applies a centromere blacklist by default.

## Hand-off pattern

When you identify a triangulated trio:

1. Confirm all three pair-match each other (not just one to the others)
2. Identify the segment's chromosome + coordinates
3. Check the trees of all three for a common ancestor candidate at the expected generation depth
4. If MRCA found, propose the hypothesis with cM-range + triangulation count as evidence
5. If MRCA not found, suggest building back the tree of the weakest/shallowest of the three until convergence

## Related

- [mrca-estimation.md](mrca-estimation.md) — turning a triangulated trio into an MRCA depth estimate
- [kinprove-connector-tools.md](kinprove-connector-tools.md) — the `get_kit_triangulations` tool
- [endogamy.md](endogamy.md) — why triangulation degrades in endogamous populations
