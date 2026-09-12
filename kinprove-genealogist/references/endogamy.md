# Endogamy in a Kinprove research study

Endogamy means repeated marriage within a population. Pedigree collapse and
multiple relationships can make shared DNA difficult to attribute to one
recent ancestral couple. Known population history and documented descent
paths provide context; a high total, many matches, or short segments alone do
not diagnose endogamy or quantify its probability.

## Keep the settings' scopes separate

| Scope | What to establish | What it does not establish |
|---|---|---|
| Source project's `dna_assignment_profile` | The source profile and effective evidence filtering resolved from runtime configuration | That all stored pairs were computed with today's settings |
| Raw detection and import | Source, detector/import settings, coordinate build, and result provenance where exposed | The thresholds used later for triangulation or hypothesis evidence |
| Triangulation | Stored engine/provider result, overlap method, and applicable pipeline thresholds | Phased inheritance or the hypothesis scorer's evidence floor |
| POI `endogamy_mode` | Persisted effective scoring mode where exposed; inspect the native expected distribution and variance adjustment | The flag's creation-time origin, a change to observed cM, an increased segment cutoff, or a new detector run |
| Hypothesis evidence | The actual minimum-segment filter, retained/excluded evidence, selected pair, and scoring context | A calibrated probability of endogamy or an automatic long-segment bonus |

The maintained backend resolves the hypothesis evidence floor from the
**source project profile**, while POI endogamy affects the scoring model,
including degree-dependent variance. These can intentionally differ. The
repository configuration has a 7 cM default hypothesis-evidence floor and a
12 cM endogamous-profile floor; these are configuration defaults for that
stage, **not runtime observations or detector/X/triangulation thresholds**.
Do not substitute this table for the connected deployment's effective values.

At POI creation, an omitted `endogamy_mode` may default from the source
profile once. The application persists the resulting boolean, not whether
the caller supplied it or it was defaulted. That origin is not retained or
exposed by the checked connector; do not describe the stored mode as live
inheritance from the source profile or reconstruct its origin from today's
settings. Without a separate creation record, its origin remains unknown.

Read effective settings and their provenance through the live connector when
available. A bare POI flag or profile label is insufficient to establish an
effective cutoff. If metadata is unavailable, say **“effective evidence
cutoff unknown through this connector.”** Current settings and stored scoring
settings may differ; even a current project setting does not certify every
pair's processing history. See the [connector limits](kinprove-connector-tools.md#capability-limits).

## Inspect the segment profile first

For each material pair, record the following where exposed. Missing fields
remain unknown; a page-local summary is not the full pair distribution.

- People and exact kit pair, including native kit-selection scope and any
  imported/raw precedence. Do not sum multiple kits or mirrored pair rows.
- Autosomal total cM, segment count, largest segment, and the full length
  distribution when available. Keep X totals/counts separate.
- Source, chromosome/build/coordinates, effective filters, excluded evidence,
  run identity, timestamps, and stale/incomplete flags.
- Whether an absent value means uncomputed/missing, measured zero,
  filtered-to-zero, X-only, or merely outside this tool's source scope.

A high sum made entirely of short segments can be compatible with background
sharing. It neither measures the probability of endogamy nor excludes a real
distant relationship. Compare the native observed evidence with the model's
expected sharing for documented and hypothetical paths. Do not mechanically
move every relationship one category farther away, and do not declare DNA
unusable beyond four generations.

## Research priority and score contribution

Inspect the longest segments and their transmission across tested ancestors,
siblings, and descendants. A longer tract in an older relative can identify a
useful investigation even if a focal descendant inherited a shorter part or
none. Shared copies along a parent/child chain are dependent observations;
do not sum them as separate confirmations. Neither a documented cousin nor a
long tract assigns that tract to a particular ancestral couple by itself.

Keep this research priority separate from native scoring. Inspect
`get_hypothesis_detail`'s `scoring.components` and `participant_fits`, and any
available triangulation trace. A displayed largest segment is not evidence
that the scorer uses it. In the checked connector, `relationship_probabilities`
explicitly accepts `largest_segment_cm` as **advisory, unused in scoring**.
The native POI participant-fit likelihood uses filtered autosomal sharing and
pedigree-aware expected distributions; do not invent a pairwise segment-length
weight. Triangulation components have their own native rules. If a component's
length weighting or calibration is unavailable, state that limit.

The [published DNA research recipe](https://github.com/Kinprove/dna-research-recipes/blob/f7d4a5f99c9a65583beb98f541ea213923602b00/dna-research-recipes/recipes/ai-for-dna-research.md#endogamous-matches-separate-prioritization-from-ancestral-attribution)
describes segment-mapping and prioritization heuristics. Its research filters
are not Kinprove defaults, a scoring formula, or corrected relationship odds.
Sorting by longest segment does not calibrate probabilities.

## Family-to-family workflow

1. Identify related focal POIs and their known relationships. Inventory the
   relevant tested relatives on both sides, including tested ancestors and
   descendants beyond the first match list. Use kit inventory plus bounded
   pedigree reads; state traversal limits and unlinked kits.
2. Group testers by documented descent paths. Retain a person reachable by
   two paths as one person with both paths. Different child branches can
   reconnect later; do not label them statistically independent by default.
3. Reuse an appropriate multi-POI study, or create one within the authorized
   mutation scope. Inspect the actual participants, kits, settings, evidence
   gaps, and currency before generating or interpreting scores.
4. Use native generation, scoring, and composite hypotheses. Inspect each
   POI's candidate and the joint placement: do siblings share the required
   parents, and is a niece/nephew attached through the correct sibling?
   A returned composite or alignment score is not proof of compatibility.
   Flag an inconsistent topology instead of rewriting the known family to
   fit it. Preserve native multipath/consanguinity adjustments without
   manually adding expected cM or treating every path as another tester.
5. Compare segment patterns across relatives and branches. Separate the
   hypothesis being scored from the actual common ancestor used for a
   participant fit (`via_mrca` and `common_ancestor_family_id` when returned).
   Neither coordinate overlap nor a model's couple label proves transmission
   through that couple.

## Authorized sensitivity experiments

Duplicate the POI study before changing its cohort or hypotheses. Record the
baseline's people, kits, source profile, scoring settings, candidate set, and
run identities. Keep the source project and original study intact.

Compare the full family with clearly labeled selections: for example,
different descendant branches, a tested ancestor instead of a descendant, or
the study with/without a tester who connects two paths. Preserve both paths in
the baseline and restore the tester's correct interpretation after the
experiment. A person on overlapping branches is not an independent panel.
Changing `participant_ids` replaces the selection and regenerates hypotheses;
use the live schema's bounded delta operations when appropriate.

Request a stricter **POI-local** filter only if the current schema supports
one and exposes what it changes. A POI copy alone does not isolate a source
project profile change. If no isolated filter is available, report that
capability gap; do not change the source project's pipeline as a substitute.
No downstream filter can recover segments lost upstream. A separately labeled
filtered research total is not a substitute native likelihood.

Read back the changed cohort and fresh native results. Compare candidate
placements and participant/component support, not just their displayed rank.
Different cohorts and candidate sets produce different conditional rankings;
their probabilities are not directly comparable. A high rank or `good` fit
is a model result, not independent confirmation of the family hypothesis.

## Related

- [Connector guide](kinprove-connector-tools.md) — evidence scopes and native ranks
- [Family example](../examples/workflows.md#workflow-4--compare-two-families-under-endogamy) — related POIs and overlapping descent
- [Triangulation](triangulation.md) — unphased interval overlap and attribution
- [X inheritance](x-dna-inheritance.md) — separate X evidence and path limits
