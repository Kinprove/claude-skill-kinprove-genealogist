# Kinprove MCP Connector — research guide

Discover the connected server's current tool descriptions and input schemas
before using this guide. Names below are MCP protocol names; a client may
expose them with a prefix such as `mcp__kinprove__`. Do not assume an inventory
size, new fields, or an unreleased tool. Use returned short-ids in the correct
project/POI/person/kit/scenario scope, not names or internal integer IDs.

## Scope and authorization

A user asking you to inspect a connected project authorizes the necessary
bounded reads. Reuse that authorization and the established project. Use
`list_projects` only when the project still needs resolving; `get_project`
returns project metadata, counts, DNA profile, and latest-check status, not
all people, kits, or hypotheses. Discover those through their scoped tools.

Read-only analysis does not authorize creating studies, recalculating data,
changing people/families, or deleting evidence. If the user has already
requested a specific POI experiment or recalculation, carry it out within that
scope without another permission loop. Honor the tool's confirmation contract
(e.g. `confirmed: true` only for an already authorized action). Otherwise
obtain the missing authorization for that concrete mutation. Check the action,
not just a `manage_` prefix: some such tools also have read actions.

Source-tree edits assert genealogy. Hypothetical ghost-tree/POI edits are
research mutations with different effects. Neither licenses the other. Keep
sensitive evidence within the authorized workspace; public examples use
wholly fictional families, not renamed real data. Authentication belongs to
the connector; never request or reproduce credentials in a research report.

## Discover people, kits, and descent paths

- `search_individuals`, `list_individuals`, `get_individual` — resolve people
  and stored details. Preserve date precision and original date text.
- `list_dna_kits` — map kits to people, provider, and compute state. Include
  unlinked kits in the inventory's limitations rather than silently dropping
  them. `list_available_pois` supplies eligible people and kit short-ids.
- `get_ancestors`, `get_relationship` — bounded ancestry and known pair paths.
  Use these to connect relevant tested people to candidate families. A name
  collision is not identity; a depth-limited search is not exhaustive.
- `get_project_tree` — tree topology when the authorized project fits its
  response limit. For larger trees, inventory kits and walk their relevant
  ancestry in bounded steps; do not invent a general descendant tool.
- `get_poi_project`, `get_poi_ancestry`, `get_participants` — inspect existing
  study membership, tree context, and participants. People may own several
  kits. Preserve both paths of a multiply related person, counting them once.

## Choose the evidence surface deliberately

| Tool | Read scope and interpretation |
|---|---|
| `get_pair_segment_evidence` | Current raw and imported evidence for two people, native selected kit pair, effective autosomal filter, counted/excluded rows, and complete distribution summaries. Optional POI scope applies that study's kit pins and pair overrides. Segment rows may be paginated/capped; this is a live extraction, not a stored scoring snapshot. See the detailed contract below. |
| `get_kit_matches` | Current project-assigned **raw** segments, aggregated for one focal kit and each counterpart kit, sorted by `total_cm`. The checked implementation sums chromosomes together: this is not an autosomal-only total. `is_canonical_match_kit` helps identify duplicate people, but does not certify the pair chosen by a POI scorer. |
| `list_dna_segment_references`, `get_dna_segment_detail` | **Imported** segment references and per-person imported match breakdowns. Detail takes `individual_id`, not `kit_id`. Provider summaries and row availability may differ. |
| `get_segment_region_overlaps` | **Imported segments only** in the checked contract. `min_cm` filters whole-segment cM, not overlap length or the POI evidence floor. Empty results do not exclude raw segments in the interval. |
| `get_kit_triangulations` | Stored engine-computed or provider-imported triads touching one kit. Read `evidence.source`, `method`, `phasing`, `parental_origin`, and X-path metadata where present. Constituent raw pairwise intervals cover eligible triads, not the complete raw pair distribution. |
| `get_dna_check_pairs` | Autosomal/X totals, counts, largest autosomal segment, source, expected sharing, and fit from an identified DNA-check run. These are stored results, not necessarily today's pair evidence or a POI's filtered evidence. |
| `get_hypothesis_detail` | Native `scoring.components`, `participant_fits`, and stored triangulation support. `observed_cm` is filtered autosomal evidence; `shared_cm_with_poi` in `get_participants` is unfiltered. Read `applicable`, `evidence_source`, and the actual common ancestor used for each fit. |

Follow pagination and declared caps. Do not describe one page as the full
cohort, distribution, or globally highest matches after a local sort. Do not
sum both directions of a segment, several kits from one person, or raw plus
imported observations of the same evidence. Native selection may use a POI kit
pin and a processed-pair fallback different from the match list's canonical
kit. If that selected pair is not exposed, record it as unknown rather than
claiming the two surfaces reconcile.

Interpret evidence states explicitly:

- `not_compared` / missing / `evidence_source: none`: no usable measurement
  established; a numeric placeholder does not turn it into a measured zero.
- `processed_zero`: a measured zero under that comparison's retained scope,
  not proof of no genealogical relationship.
- Below-filter evidence: measurement exists but nothing passes the stated
  filter; distinguish this from uncomputed data. If provenance is insufficient
  to tell, say so.
- X-only: autosomal zero can coexist with X sharing; inspect both components.
- Empty imported read: no rows in that imported scope, with raw evidence still
  unknown. A shared-match row alone is not positive DNA evidence; inspect the
  underlying measurements and currency.

## Current pair segment evidence

Where the deployment exposes `get_pair_segment_evidence`, pass
`project_uuid`, `individual_uuid`, and `match_individual_uuid`. These identify
two distinct people in the source project. `individual_uuid` is the focal
side and wins precedence ties; keep that orientation consistent. Adding
`poi_project_uuid` requires that focal person to be a scoped POI of that
study and applies the study's kit pins, local filter, and pair-override
overlay. Omitting it uses project scope without those POI overrides.

Read these fields together:

- `kit_selection`: the selected pair, pins, and considered kit candidates.
  `segments` can include rows from other considered pairs marked
  `not_selected_kit_pair`; those are not extra scored observations. Raw
  precedence can also leave imports marked `superseded_by_raw`.
- `effective_filters`: applied `min_segment_cm`, project baseline,
  `min_segment_cm_scope`, and chromosome scope. The floor filters autosomal
  evidence; X is reported separately and is not used in the autosomal fit.
- `evidence`: the native effective summary and its source. Check
  `pair_override.type` / `applied` where present; an applied user override is
  not detector evidence. Rows and distribution describe the extraction
  before that overlay, so do not replace the effective summary with a sum.
- `distribution`: separate buckets for counted autosomes, X, below-floor
  rows, superseded imports, and unselected raw kit pairs. Its
  `summary_scope: complete` covers the extraction, even when the segment list
  is incomplete. Empty buckets have zero count and null summary lengths;
  use the evidence source to distinguish missing from measured-zero data.
- `segments_scope`: `complete`, `capped`, or `page`. When individual lengths
  or intervals are needed, use `page` / `per_page` and follow `has_more` to
  completion. A complete summary does not make a capped list complete.
  Null row `genome_build` means the coordinate build is unestablished for
  that row; do not infer it from a different kit or imported view.
- `basis: current_data_and_settings` and `context`: in POI mode, inspect
  `participant_in_scoring_set`, `latest_scoring_run_id`, and stale fields.
  A readable pair need not participate in the study's score. Retain the read
  time; pagination is not an immutable run export, so re-read if data/settings
  change during collection. A current extraction does not certify what an
  older hypothesis run consumed.

## Native calculations and result currency

`get_dna_check_summary` and `list_dna_check_runs` establish available check
results. `run_dna_check` scores existing DNA evidence against the tree; it is
not the raw IBD detector and does not fill missing raw comparisons. Start or
refresh it only within authorized compute scope.

`get_dna_check_pairs` is ordered by z-score, with optional `individual_id`
filter. Retain `run.id` and each row's `dna_check_run_id`; compare run identity
across pages. If a new run replaces results during pagination, restart at
page 1 as directed by the tool. Do not join an old largest segment to a new
raw total as if they were one observation.

For native hypothesis work:

1. Read `list_poi_projects` and the selected study's detail/analysis,
   participants, evidence gaps, stale flags, and current schemas. The
   `get_poi_project_analysis.sections` and `get_hypothesis_detail.sections`
   enums constrain available projections; requesting an absent section can
   return an omitted section, not a negative finding.
2. Within authorization, `create_poi_project` takes `project_uuid` and
   `pois: [{individual_id, dna_kit_id?}, ...]`; no anchor is required.
   `participant_ids` identifies people and defaults to auto-resolution of
   DNA-connected individuals when omitted. Read the resolved selection back.
   Creation also attempts initial branch generation: inspect
   `project_persisted`, `generation.status`, its reason/error, and
   `next_action`. Successful creation-time generation points to
   `rescore_hypotheses` because those branches are not yet scored. Follow that
   action within authorization instead of generating the same branches again.
   A persisted study with failed or empty generation is not a failed create;
   use its returned UUID and resolve the reported cause rather than creating
   another study.
3. Generate only when initial branches still need generating or an authorized
   topology rebuild is required. `check_hypothesis_readiness` precedes that
   `generate_hypotheses` call. Generation creates/replaces ghost hypotheses
   and can clear prior branches. Inspect its outcome and scoring currency;
   reuse existing fresh results when the request is inspection only.
4. `list_hypotheses` and `get_hypothesis_detail` expose candidates and native
   support. Inspect `scoring.components` and participant fits; `via_mrca`
   means the fit can use a different common ancestor than the scenario's
   anchor. `triangulation_trace` is an optional heavier diagnostic; agreement
   with stored aggregates does not verify historical provenance. Its
   `cross_validation_score` reuses pair cM, not held-out validation.
5. `list_composite_hypotheses` combines related POIs; inspect the returned
   search limits and known-relationship compatibility. Increasing `top_n`
   changes the number returned, not necessarily candidate search depth.
   `get_ghost_tree` supplies topology; materialization is a separate mutation.
6. `compare_hypotheses` compares native support within one POI study.
   `rescore_hypotheses` recalculates within authorized scope. Read back
   results and settings after mutations instead of trusting an old rank.

The backend owns calculations for both frontend and MCP. Compare the same
scenario, POI, evidence, and scoring context. `rank` is the per-POI scoring
rank; ghost-tree `display_rank` is a couple-grouped presentation position and
can differ. Composites can have no persisted scoring rank. `scoring_run_id`
on ghost-tree/scenario reads names the project's latest scoring run, not
necessarily each scenario's provenance after a single-POI rescore. Equal IDs
show no intervening scoring pass, not that every result shares one snapshot.

`relationship_probabilities` is a native cM-based alternative comparison,
not the tree-aware POI engine. Its checked schema says `largest_segment_cm`
is advisory and unused in scoring. Do not infer a length weight from a
returned largest-segment field. Native `good`, likelihood, probability, and
rank remain conditional model outputs; do not add custom bonuses or compare
probabilities across different cohorts/candidate sets as the same quantity.

## Capability limits

The maintained implementation checked on 2026-09-13 includes the pair-evidence
tool above, `effective_scoring_settings` and `scoring_settings_at_generation`
on `get_poi_project` and the `project` section of `get_poi_project_analysis`,
and optional `evidence_min_segment_cm` on
`create_poi_project` / `update_poi_project`. Settings changes can add a
`settings` stale reason. Discover the connected deployment's actual schemas
and returned fields before relying on them; a repository capability is not
proof that every server has deployed it.

Read the [endogamy settings contract](endogamy.md#keep-the-settings-scopes-separate)
for scope/source fields and recorded settings. These expose current values
and a limited scoring-settings record, not a complete historical detector,
input, or model snapshot. Null or mixed recorded settings remain a provenance
limit; a current pair read cannot reconstruct missing run history.

On older connections without these capabilities, a largest segment from a
DNA-check run, imported region rows, or stored triad intervals cannot fill the
gaps for every current raw pair. Use a narrowly scoped native export or
authorized application read when available; otherwise retain the specific
unknowns and useful tree/native-score analysis. Do not request broad raw
exports or treat missing filter metadata as the default threshold.

## Preserve the original during experiments

Use `duplicate_poi_project` for an authorized sandbox copy. It copies POI
research data; it does not create an independently filtered source project.
`update_poi_project.pois` rebuilds tree and hypotheses; `participant_ids`
replaces the entire selection and regenerates hypotheses. The schema also
supports bounded `add_participant_ids` / `remove_participant_ids` deltas.
Read back the copy and its resulting cohort before comparing native support.

`manage_pair_overrides` is a mutation: `zero_match` asserts user-confirmed
zero evidence, while `ignored` removes an otherwise non-positive pair's
weight. Neither is a substitute for an uncomputed comparison; do not add an
override just to make a missing pair look measured. Positive evidence takes
precedence. Source-tree changes, kit reassignment, clearing, deletion, and
composite materialization each need authorization for their actual scope.

## Related

- [SKILL.md](../SKILL.md) — working method and concise hand-off
- [Workflows](../examples/workflows.md) — tool sequences and fictional examples
- [Endogamy](endogamy.md) — filtering, related testers, and sensitivity experiments
- [Triangulation](triangulation.md) — interval evidence and attribution limits
