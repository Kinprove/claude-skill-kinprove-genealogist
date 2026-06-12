# X-chromosome inheritance

X-DNA follows different inheritance rules from autosomal DNA. Used correctly, it dramatically narrows hypothesis space. Used carelessly, it produces false confidence.

## The basic rules

- **Males** inherit one X from their mother only (their Y comes from their father).
- **Females** inherit one X from each parent.
- **A father transmits NO X-DNA to his sons.** This is the most important rule.
- A father transmits his single X (which he got from his mother) to all his daughters.

So: starting from a male, the X-DNA can only have come from:

- His mother
- Through his mother, from either of HER parents — her mother or her father. Any X that came from her father originated as that father's single X, which he in turn inherited from HIS mother. Every female has a father by definition, so there is no "only if" condition here.

## The X-inheritance chart

The X-only ancestry tree of a male:

- Mother (full X contribution)
- Mother's mother + mother's father (his mother) — partial through recombination
- Continue up only through females or through females and their fathers' mothers

You can construct an "X-only ancestor list" by following these rules backward. For a typical male, his X-only ancestors are a sparse subset of his full ancestor list. For example, his paternal grandparents contribute zero X.

## Why this matters for hypothesis work

If two people share significant X-DNA, the MRCA must lie in BOTH of their X-only ancestor sets. This often eliminates entire branches of the tree from consideration.

Example: two males share a substantial X-segment. The MRCA cannot be on either male's paternal grandfather's line. That excludes ~25% of the typical tree.

## When X-evidence helps

- Distinguishing maternal vs. paternal side hypotheses
- Confirming a hypothesis when the proposed MRCA is on a valid X-path
- Ruling out hypotheses where the proposed MRCA is paternally-excluded

## When X-evidence misleads

- **Low cM thresholds.** X-segments below ~10 cM are noisy and over-reported. Kinprove uses `MIN_CM_X = 6.0` (lower than autosomal because X is shorter) but raises to 10.0 in endogamy mode.
- **Endogamy.** Pseudo-X-triangulations form easily.
- **Recombination is sparse on X** — segments tend to be longer when they exist, but the chance of an X-segment surviving past 3–4 generations is lower than autosomal.

## Kinprove platform behavior

X-DNA is **reported separately** in Kinprove:

- `shared_cm_total` = autosomal only (industry standard)
- `shared_cm_x` + `segment_count_x` = X-specific
- X-segments enter the triangulation input → produce `DnaTriangulatedSegment` with `chromosome='X'`
- X-triangulation bonus in hypothesis scoring is **additive-only** (never penalizes) — requires valid X-path via participant fits

Tools (when the connector is active; MCP protocol names, surfaced as `mcp__kinprove__<name>`):

- `get_xdna_analysis` — X-specific analysis for a POI project (5 actions: analysis, impossible_matches, validations, reachable_ancestors, descendants)
- `get_kit_triangulations` — returns triangulations including X-chromosome trios

## FTDNA caveat

5 of 17 FTDNA kits in test data are missing X-body SNPs (kits 115, 116, 120, 129, 134 per Kinprove's gotcha records). FTDNA exports a header for the X-chromosome but no body data. When working with FTDNA kits, confirm X-data is present before relying on X-evidence.

## Hand-off pattern

When invoking X-evidence:

1. Confirm the X-segment is above threshold (>10 cM, ideally >15 cM in endogamy)
2. Verify the X-inheritance path is valid for the proposed MRCA from both people's perspectives
3. State explicitly: "The X-evidence is consistent / inconsistent with this hypothesis"
4. Use X to narrow hypothesis space, not as standalone proof

## Related

- [triangulation.md](triangulation.md) — X-chromosome triangulation and its additive-only scoring
- [endogamy.md](endogamy.md) — why X thresholds tighten in endogamy mode
- [kinprove-connector-tools.md](kinprove-connector-tools.md) — the `get_xdna_analysis` tool
