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
| POI `endogamy_mode` | Persisted scoring mode, including degree-dependent variance. It does not by itself increase the hypothesis-evidence floor or change observed cM. |
| `imported_triad_min_segment_cm` | Separate imported-triangulation floor following the POI endogamy mode. It is not the pair-evidence floor. |
| `raw_detection_min_segment_cm` | Current profile-derived detection setting with `applies_to: same_provider_pairs`. It does not certify historical detector settings or trigger reprocessing. |
| Raw/imported segment and triangulation provenance | Read the available detector/import metadata, coordinate build, and engine/provider method separately. A configured floor or stored triad does not establish phase or parental origin. |

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

The separate `relationship_probabilities` tool accepts `largest_segment_cm`
as **advisory, unused in scoring** in the maintained schema. A displayed long
segment can guide research without receiving a native score bonus. Inspect
any triangulation component or trace at its own stated strength; do not add
length weights, sum multipath expectations manually, or turn a `good` fit
into independent confirmation. Report unavailable component calibration as
unknown.

## Authorized sensitivity experiments

Preserve the baseline and use `duplicate_poi_project` for authorized cohort,
generation, or filter comparisons from the common recipe. The copy retains
its source project; it does not isolate a source-profile change. Record the
actual people, kits, settings, candidate set, and run context for each copy.
Changing `participant_ids` replaces the selection and regenerates hypotheses;
use bounded add/remove operations where appropriate and read membership back.

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
