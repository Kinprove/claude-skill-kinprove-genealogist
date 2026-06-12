# Endogamy

A genealogical population is **endogamous** when its members repeatedly marry within the group over many generations, causing each person to share multiple ancestors with most others in the population. Classic endogamous populations: Ashkenazi Jewish, Acadian/Cajun, Amish/Mennonite, Polynesian, some Mexican and Newfoundland populations.

## Why it matters

DNA matches from endogamous backgrounds look misleading:

- **Inflated cM totals** — a 1C-shaped 800 cM match might actually be a 2C–3C because the two people share multiple ancestors via different paths
- **Matches at every cousin level** — instead of clear category clusters, you get a continuous spectrum
- **Spurious triangulations** — three people from the same endogamous population will triangulate at many loci without a unique MRCA
- **Predicted relationships skew too close** — testing-company predictions consistently overshoot for endogamous matches

## Detection signals

If two or more of these fire, the user's matches may be endogamous:

1. Many "1C2R or closer" predicted matches that the user doesn't recognize
2. Total match cM dwarfs comparable non-endogamous trees (e.g., 500+ matches >100 cM)
3. Tree research already shows distant cousins on multiple lines
4. Population background is one of the known endogamous groups
5. Triangulated segments at many independent loci without converging on a single MRCA candidate

## What to do about it

1. **Lower confidence in single-pair cM predictions.** Use a category range that's one step further out than the cM table suggests.
2. **Demand multi-line evidence.** A hypothesis should converge from multiple independent matches, not just one inflated pair.
3. **Use Kinprove's endogamy mode** — it applies tighter cM cutoffs (MIN_CM_X_ENDOGAMY = 10.0 vs. 6.0 in non-endogamy), centromere blacklist, and adjusted scoring weights. Set via the POI project's endogamy mode toggle.
4. **Prefer recent paper-trail evidence.** Endogamy + DNA gets unreliable past 4 generations; lean on documentary research for deeper ancestors.

## Kinprove technical details

Kinprove's IBD detection thresholds (per the platform's DNA pipeline knowledge base):

- `MIN_CM = 7.0` — autosomal minimum in the default (non-endogamy) profile
- `MIN_CM_ENDOGAMY = 10.0` — endogamy mode **raises** the autosomal minimum to filter population-level IBS noise
- `MIN_CM_X = 6.0` — X-chromosome minimum (lower than autosomal because X is shorter)
- `MIN_CM_X_ENDOGAMY = 10.0` — endogamy mode raises the X minimum
- Centromere blacklist enabled
- 4-layer false-positive defense: gap splitting → tiered blacklist → MIN_CM → MR filter (7–15 cM)

Endogamy operates at two scopes: the **project** `dna_assignment_profile`
(`default` vs `endogamous`) drives the IBD detection + evidence-filtering
thresholds, while the **per-POI-project** endogamy-mode toggle (since
2026-04-12) drives the hypothesis-scoring variance. Endogamy-mode scoring also
widens expected distributions per relationship degree rather than adjusting the
observed cM values.

## Related

- [cm-ranges.md](cm-ranges.md) — the baseline cM ranges endogamy inflates
- [triangulation.md](triangulation.md) — why triangulation degrades under endogamy
- [x-dna-inheritance.md](x-dna-inheritance.md) — the X thresholds that tighten in endogamy mode

## Hand-off pattern

When endogamy is suspected:

1. Name it explicitly: "This looks like an endogamous match population"
2. Cite which signal(s) triggered the diagnosis
3. Suggest enabling endogamy mode on the POI project if it isn't already
4. Pivot research focus from DNA-alone to documentary records + multi-line corroboration
5. Reset relationship-prediction expectations downward
