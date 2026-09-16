# Endogamy in a Kinprove research study

Use the [general family-comparison recipe](https://github.com/Kinprove/dna-research-recipes/blob/253195738dc9d2d0722b1520d75a37f150ae9a3e/dna-research-recipes/recipes/ai-for-dna-research.md#compare-families-across-branches-and-generations)
for the method, evidence caveats, and fictional teaching case. This reference
maps that work to Kinprove settings and native results. The recipe's research
filters are not Kinprove defaults, a scoring formula, or corrected odds.

## Keep the settings' scopes separate

Discover the connected deployment's schemas first. The maintained connector
exposes `effective_scoring_settings` through `get_poi_project` and the
`project` section of `get_poi_project_analysis`. Each setting reports its
value, scope, and source; read those fields rather than guessing from a label.

| Setting or evidence stage | Kinprove interpretation |
|---|---|
| Source project's `dna_assignment_profile` / effective `dna_profile` | Resolves the project hypothesis-evidence floor and other stage-specific defaults. Today's profile does not establish a stored pair's processing history. |
| `evidence_min_segment_cm` | Hypothesis-evidence floor: the greater of the project floor and a POI-local cutoff. Inspect `value`, `project_value`, `poi_value`, `scope`, and `source`. |
| `scored_triangulation_min_segment_cm` | The same effective floor applied to scored autosomal triangulation rows (`applies_to: autosomal_rows`), with the same `value`, `scope`, and `source` as `evidence_min_segment_cm`. Shorter rows appear in `triangulation_trace` as excluded, never as support. |
| POI `endogamy_mode` | Persisted scoring mode, including degree-dependent variance. It does not by itself increase the hypothesis-evidence floor or change observed cM. |
| `imported_triad_min_segment_cm` | Separate imported-triangulation floor following the POI endogamy mode. It is not the pair-evidence floor. |
| `raw_detection_min_segment_cm` | Current profile-derived detection setting with `applies_to: same_provider_pairs`. It does not certify historical detector settings or trigger reprocessing. |
| Raw/imported segment and triangulation provenance | Read the available detector/import metadata, coordinate build, and engine/provider method separately. A configured floor or stored triad does not establish phase or parental origin. |
| `endogamous_preset` (where exposed) | The configured **Endogamous — minimum N cM** preset. `min_segment_cm` is N; `applied` is true only when POI endogamy mode is on and the effective hypothesis-evidence floor is at least N. Endogamy on with a lower floor is a custom setup, not the preset. |

To set up an endogamous study deliberately, send `scoring_preset: "endogamous"`
on `create_poi_project`, or alone on `update_poi_project`. It turns the POI
endogamy mode on and raises the POI hypothesis-evidence floor to at least N,
keeping a stricter existing floor. For a controlled experiment (for example
endogamy on at the project's 7 cM floor), set `endogamy_mode` and
`evidence_min_segment_cm` explicitly instead; the two approaches cannot be
mixed in one call. Either way, read `effective_scoring_settings` afterwards and
rescore before interpreting results. The preset sets only those two
settings: the hypothesis-evidence floor, and the endogamy mode with its usual
effects, including the separate `imported_triad_min_segment_cm`. That floor
also applies to scored autosomal triangulation
(`scored_triangulation_min_segment_cm`), but X triangulation keeps its own
rules, so do not describe the preset as removing all short-segment support.

At POI creation, an omitted `endogamy_mode` may default from the source
profile once. The application persists the resulting boolean, not whether
the caller supplied it or it was defaulted. Without a separate creation
record, that origin remains unknown; it is not live inheritance from today's
source profile.

Compare current effective settings with `scoring_settings_at_generation`
and `hypotheses_stale_reasons`. The recorded object covers scoring mode and
evidence floor, not every detector/model/input setting. A null record means
unrecorded settings; `{mixed: true}` means persisted scores do not share one
recorded settings pair. A false stale flag does not fill missing history.
See [connector capability limits](kinprove-connector-tools.md#capability-limits)
for older deployments and current-read versus stored-run provenance.

## Read evidence in the scoring study's scope

Use `get_pair_segment_evidence` where exposed. Its optional
`poi_project_uuid` selects POI kit pins, pair overrides, and the effective
local cutoff; `individual_uuid` must then be a POI of that study. Project-only
mode is useful for other relatives but is not automatically the same evidence
as a POI fit. Read selected kits, filter scope, evidence source, and complete
summary buckets before comparing results. Follow all segment pages when the
length list is needed; do not sum excluded kit pairs or superseded imports.

This tool extracts **current** evidence. Reconcile its run/staleness context
with the hypothesis being discussed rather than calling it a historical
scoring export. Preserve uncomputed, measured-zero, filtered-to-zero, X-only,
and source-limited empty states. Missing build or processing metadata stays
unknown. The [connector guide](kinprove-connector-tools.md#current-pair-segment-evidence)
explains the response fields, pagination, and pair-override overlay.

## Inspect native support and family placement

Use related POIs and their actual participant selection to apply the common
family recipe. The [Kinprove family workflow](../examples/workflows.md#workflow-4--compare-two-families-under-endogamy)
provides the tool sequence. Preserve all known descent paths and inspect
whether composite topology respects the supplied sibling/parent/niece links;
a returned composite or alignment score alone does not establish that.

Inspect `get_hypothesis_detail`'s `scoring.components` and `participant_fits`.
The native participant-fit likelihood uses filtered autosomal sharing and
pedigree-aware expected distributions. A fit's `via_mrca` and
`common_ancestor_family_id` may name a different common ancestor from the
scenario's anchor. Neither label attributes a segment to that couple.
A multipath fit's `multipath_calculation` is the native explanation of its
expected sharing: a detected path count alone does not raise the expectation,
because distant extra routes can carry zero weight.

The separate `relationship_probabilities` tool accepts `largest_segment_cm`
as **advisory, unused in scoring** in the maintained schema. A displayed long
segment can guide research without receiving a native score bonus. Inspect
any triangulation component or trace at its own stated strength; do not add
length weights, sum multipath expectations manually, or turn a `good` fit
into independent confirmation. Report unavailable component calibration as
unknown.

## Check which pedigree paths were scored

The maintained fit selects one nearest shared ancestor or couple, then looks
for the participant's additional routes to those selected ancestors. This
does not establish a joint calculation over every distinct shared ancestor
or repeated route on the POI side. A pedigree can contain two proposed
parental lines while a returned fit reflects only one of them.

For such models, enumerate the relevant routes from both people in the saved
tree. Compare them with the fit's selected common ancestor, depths,
`expected_cm`, `multipath_adjusted`, `path_count`, and `multipath_calculation`.
A null trace can occur in a fresh fit as well as an older snapshot; it does
not prove that the tree has only one route. Report the specific unaccounted
route when demonstrated, without claiming that all native multipath support
is absent or adding the paths' expected cM yourself.

The scenario's anchor is a separate source of sensitivity. The same endpoint
and parent links can return identical pair expectations but different total
scores under different anchor couples: triangulation support, age priors,
and half-relationship penalties can change. For an authorized anchor control,
hold the pedigree, cohort, kits, filters, and evidence constant, then inspect
the native components and `triangulation_trace`. If the ordering changes with
the anchor, report that instability; do not identify a parent from that rank.
Matching a trace to its stored score verifies internal agreement, not that
all pedigree paths were counted or that the biological model is calibrated.

## Authorized sensitivity experiments

Preserve the baseline and use `duplicate_poi_project` for authorized cohort,
generation, or filter comparisons from the common recipe. The copy retains
its source project; it does not isolate a source-profile change. Record the
actual people, kits, settings, candidate set, and run context for each copy.
Changing `participant_ids` clears stored scores and regenerates the automatic
branches unless the project has hand-made hypothesis work, which is kept
instead (`hypothesis_effect: preserved`); use bounded add/remove operations
where appropriate and read membership back.

Where exposed, `create_poi_project` / `update_poi_project` accept
`evidence_min_segment_cm` for that POI study only. A value must meet the
project floor and the server's validation limits; `null` clears the local
cutoff and restores the project floor. An update marks hypotheses stale
until rescored and does not reprocess DNA. Read the effective value back,
perform the authorized rescore, and inspect its resulting settings record.
A downstream filter cannot recover segments lost upstream.

If no POI-local filter is available, report the gap; do not substitute a
source-project change. A manually filtered research total is not a new native
likelihood. Compare placements and component/participant support across
copies, preserving the common recipe's dependence and cohort caveats.
Source-tree edits remain separate from these study mutations.

## Related

- [Connector guide](kinprove-connector-tools.md) — evidence scopes and native ranks
- [Family example](../examples/workflows.md#workflow-4--compare-two-families-under-endogamy) — applying the general recipe through related POIs
- [Triangulation](triangulation.md) — unphased interval overlap and attribution
- [X inheritance](x-dna-inheritance.md) — separate X evidence and path limits
