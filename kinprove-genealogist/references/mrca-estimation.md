# MRCA (Most Recent Common Ancestor) generation depth estimation

Given a shared cM amount, estimate how many generations back the MRCA likely lies.

## Quick reference

"Depth" = generations back to the MRCA from EACH tested person. The MRCA is the convergence point both individuals descend from. This is symmetric only for cousin-type relationships (Nth cousin = both N+1 generations from the MRCA). Aunt/uncle/niece/nephew rows are intrinsically asymmetric and are listed in the closest depth row that applies to one party.

| Shared cM | Cousin-row depth (both sides) | Symmetric relationship | Asymmetric / mixed-depth relationships at this cM band |
|---|---|---|---|
| 2200–3700 | n/a | parent/child, full sibling | — |
| 1100–2400 | 1 (grandparent ↔ self) | grandparent / grandchild, half sibling | aunt/uncle ↔ niece/nephew (mixed depths 1 vs 2) |
| 500–1300 | 2 (1C ↔ MRCA = grandparents) | 1st cousin, half 1C | great-grandparent ↔ great-grandchild, half aunt/uncle ↔ half niece/nephew |
| 200–600 | 3 (2C ↔ MRCA = great-grandparents) | 2nd cousin | 1C1R, half 2C |
| 60–250 | 4 (3C ↔ MRCA = 2× great-grandparents) | 3rd cousin | 2C1R, 1C2R |
| 20–110 | 5 (4C ↔ MRCA = 3× great-grandparents) | 4th cousin | 3C1R, 2C2R |
| 0–60 | 6+ | 5th cousin and beyond | may not register at autosomal cM ranges |

## Better: use ranges, not averages

A 250 cM match could be:

- 2C in average range (avg 229)
- 1C2R lower-end (1C2R avg 433 but range 102–980)
- 2C1R upper-end
- Half-2C upper-end

In practice you cannot distinguish these from cM alone. **Use tree convergence to confirm**, not cM math.

## Generations and shared cM math

Each MRCA generation back, the average expected shared cM roughly halves (autosomal). So, using depth = generations from each person back to the MRCA:

- depth 1 (grandparent / grandchild, half sibling): ~1759–1766 cM
- depth 2 (1C, half-1C): ~433–866 cM (avg 866 for 1C; ~433 for half-1C)
- depth 3 (2C): ~229 cM
- depth 4 (3C): ~73 cM
- depth 5 (4C): ~35 cM
- depth 6 (5C): ~25 cM

But variance grows with depth, so MRCA estimation from cM alone is noisy past depth 3.

## Kinprove platform support

When you have the Kinprove connector, the platform's hypothesis engine already does MRCA estimation across the user's tree using genetic + tree evidence combined. Tools (MCP protocol names; Claude surfaces them as `mcp__kinprove__<name>`):

- `generate_hypotheses` — runs the full hypothesis pipeline for a POI project
- `list_hypotheses` — lists candidate MRCAs already generated, ranked by score
- `get_hypothesis_detail` — pulls the evidence paths and triangulation support for a specific hypothesis

Prefer these over hand-math when the data is in Kinprove. See `kinprove-connector-tools.md` for the full catalog and the POI-project prerequisite for `generate_hypotheses`.

## Hand-off pattern

When estimating an MRCA:

1. Give a relationship-category range (not a single point estimate)
2. Cite the cM range explicitly with the Shared cM Project source
3. Identify what tree work would narrow the prediction (e.g., "build out the maternal grandfather's parents to see if they match the proposed MRCA name")
4. Flag uncertainty — call out endogamy, multiple-path possibilities, or NPE risks

## Related

- [cm-ranges.md](cm-ranges.md) — the Shared cM Project ranges this estimation depends on
- [triangulation.md](triangulation.md) — triangulated evidence that confirms an MRCA candidate
- [kinprove-connector-tools.md](kinprove-connector-tools.md) — the hypothesis-engine tools that automate MRCA estimation
