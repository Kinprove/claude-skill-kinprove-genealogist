# Example workflows

Use the [live connector guide](../references/kinprove-connector-tools.md) for
schemas, scopes, pagination, and mutations. Kinprove research is scoped to
kits and evidence in the authorized project; these tools do not search a
global testing-company match database. All names and numbers in the worked
fixtures below are wholly fictional, not pseudonyms or production results.

## Workflow 1 — Group project matches by a candidate ancestor

1. Reuse the authorized project; resolve it with `list_projects` only if
   necessary. Read `get_project` for metadata and `list_dna_kits` for the
   person/kit inventory.
2. Read existing `get_dna_check_summary` / `get_dna_check_pairs` for native
   autosomal/X evidence and run identity. The pairs are ordered by z-score,
   not shared cM. Complete the relevant pages before any overall ranking.
   Use `get_kit_matches` for current raw-kit leads, preserving its different
   source and chromosome scope. Do not launch computation merely to inspect.
3. Inspect segment profiles using available source-appropriate reads. Mark
   unavailable lengths, filters, selected kit pairs, or currency as unknown.
4. Use bounded ancestry/relationship reads to group people by documented
   descent, retaining all paths for multiply related testers. Shared-match
   networks can bridge distinct family lines. Check that pair rows contain
   positive evidence; ICW is not phased triangulation.
5. Use `get_kit_triangulations` as additional interval evidence with its
   stated method/provenance. Follow [endogamy.md](../references/endogamy.md)
   when a large total may reflect many short segments or multiple paths.
   Neither a cluster nor an overlap assigns an ancestor by itself.

**Without the connector:** use the provided match export, segment/source
metadata, and relevant tree branches. Identify missing inputs and unavailable
native calculations. Published [cM ranges](../references/cm-ranges.md) describe
overlapping possibilities, not unique ancestor assignments.

**Hand-off:** “This group is worth investigating through the proposed family;
its descent paths are documented, but the shared segments are not yet
attributed to that couple. Next, verify the missing link in the shallowest
branch.” Keep the full grouping table optional.

## Workflow 2 — Investigate an MRCA candidate

1. Read `get_kit_triangulations` if segment evidence is relevant. Distinguish
   engine interval overlap, provider-reported triads, and actual phasing.
   All-three pair overlap is stronger than two-of-three ICW but does not
   establish a phased shared haplotype or name an MRCA.
2. Inspect an existing suitable POI study and its currency. For an authorized
   new study, `create_poi_project` takes `project_uuid` and a `pois` array;
   it requires no anchor couple. Read the resolved participants back.
3. If generation is authorized, `check_hypothesis_readiness` precedes
   `generate_hypotheses`; generation replaces existing ghost work. Otherwise
   inspect the stored candidates without changing them.
4. Read `list_hypotheses`, `get_hypothesis_detail`, and the relevant evidence
   paths. Compare native expected sharing with observed filtered evidence,
   actual common-ancestor paths, dates, and missing records. Check related
   POIs jointly using the family workflow below.

**Without the connector:** compare the supplied trees, records, and segments.
State the candidate and missing evidence; do not fabricate a native score.
Use [MRCA estimation](../references/mrca-estimation.md) to express separate
path depths rather than one assumed symmetric generation number.

**Hand-off:** “The couple is a candidate under the current tree and model.
The overlap does not establish its parental origin. Next, verify the record
connecting the least-supported descendant branch.”

## Workflow 3 — Interpret an unfamiliar close match

For a fictional 1,400 cM match, first establish whether that number is
**autosomal**, its source, and the people/kit identities. Inspect counts,
largest segment, and chromosome evidence where available. Imported detail
uses `get_dna_segment_detail` with a person ID; it is not a raw-kit lookup.
Use native relationship calculations and known tree paths when connected.

[Shared cM Project v4 ranges](../references/cm-ranges.md) include overlapping
possibilities such as half sibling (1160–2436 cM), grandparent/grandchild
(984–2462), and aunt/uncle–niece/nephew (1201–2282). These ranges do not
identify the relationship. Compare ages, documented paths, appropriate
IBD1/IBD2 evidence if available, and [X paths](../references/x-dna-inheritance.md).
Missing X evidence does not exclude a relationship. An unfamiliar name alone
is not proof of misattributed parentage; discuss that possibility sensitively
only when the evidence makes it relevant.

**Without the connector:** use the same evidence supplied by the user;
identify missing identity, chromosome, or tree information.

**Hand-off:** give the supported shortlist, the material uncertainty, and the
one observation or record that would best distinguish the alternatives.

## Workflow 4 — Compare two families under endogamy

Use the inputs and research interpretation in the [common fictional family case](https://github.com/Kinprove/dna-research-recipes/blob/253195738dc9d2d0722b1520d75a37f150ae9a3e/dna-research-recipes/recipes/ai-for-dna-research.md#fictional-worked-comparison).
The family map and segment table are maintained there. The sequence below
shows their Kinprove application, assuming the user authorized inspection
and copied POI studies for generation and sensitivity comparisons. It does
not assert that the fictional data are present in a connected project.

1. Resolve the source project and person/kit short-ids. Read `list_dna_kits`,
   bounded ancestry/relationships, the existing POI study, and participants.
   Map Ada, Boris, and Celia to the focal POIs; preserve their known family
   links, Irma's older generation, Dana's two paths, and Mara's two kits.
   Record unlinked kits and unavailable family comparisons as coverage gaps.
2. Read `effective_scoring_settings`, recorded scoring settings, and stale
   reasons where exposed. The common case's 7 cM reporting floor is an input
   fact, not a value established by a Kinprove profile or POI flag. Verify
   any native floor independently before claiming the observations reconcile.
3. For a focal POI's material pair, call `get_pair_segment_evidence` with
   `project_uuid`, `individual_uuid` (the focal person),
   `match_individual_uuid`, and `poi_project_uuid`. These are returned person
   and study short-ids, not kit IDs. Page through `segments` as needed; read
   `kit_selection.selected_pair`, `effective_filters`, `evidence`, and the
   complete `distribution` buckets. Verify the selected Mara kit rather than
   assuming it from the raw match list. Do not add her other kit's rows.
4. Irma is not one of the baseline focal POIs. Read her pairs in project mode
   if that meets the question, labeling that different scope; for a native
   older-generation comparison, create/reuse an authorized copied study with
   Irma as a POI and read its own effective settings and evidence. Passing her
   as `individual_uuid` in the original study's POI mode is invalid. An
   arbitrary returned pair need not be a scoring participant: inspect
   `context.participant_in_scoring_set` as well as actual membership.
5. In the authorized working study, retain all three baseline POIs using
   `create_poi_project.pois: [{individual_id, dna_kit_id?}, ...]` when creating
   it, or the existing study when suitable. Read back actual participants;
   automatic selection does not guarantee the complete family inventory.
   Run `check_hypothesis_readiness` before authorized `generate_hypotheses`.
6. Read each POI's `list_hypotheses` / `get_hypothesis_detail`, then
   `list_composite_hypotheses`. Inspect components, applicable fits, actual
   MRCA paths, and treatment of multiple paths. Check the returned topology
   against the common case's known parent links. If that requires
   `materialize_composite_hypothesis`, use the authorized copy and inspect
   `get_ghost_tree`. Reject inconsistent placements; leave the source family
   intact. No tool output here is supplied or implied by the teaching case.
7. Apply the common recipe's branch/generation sensitivity checks through
   `duplicate_poi_project`, reading back each cohort and native result. For
   an authorized stricter filter, use the copy's `evidence_min_segment_cm`
   only when exposed, verify the effective value, and rescore. Do not change
   the shared source profile to simulate a local setting. A new pair read
   reflects current data and settings, not necessarily the stored score.

**Without the connector:** apply the common case or the user's actual study
exports qualitatively. Native ranks, component weights, and composite
validation remain unavailable. If the external case cannot be read, do not
reconstruct its missing measurements from names or this tool sequence.

**Hand-off:** give the research lead and uncertainty from the actual evidence,
then add the Kinprove study's material limitation: for example, a stale score,
a selected-kit mismatch, or an uncomputed pair. Do not present the common
case's conclusion as a result obtained from the user's project.

## Workflow 5 — An empty region read on an older connector

**Fictional connector state:** `get_kit_matches` returns a current raw-kit
aggregate; `get_segment_region_overlaps` returns no imported rows in the
requested interval. A stored DNA-check run has an older largest segment.
The connected deployment lacks `get_pair_segment_evidence` and effective
hypothesis-filter metadata. This is an older-capability fallback, not a claim
that the maintained connector lacks those features.

Do not splice the current total and old maximum into one autosomal profile.
The empty imported view does not exclude raw overlap. First discover whether
the current connection now offers the pair-evidence tool; if so, use its
source/POI scope and current-run limitations as described above. Otherwise
use a bounded native export or authorized application read if available;
retain unknown lengths, effective filters, and selected scoring pair when it
is not. Do not launch recalculation or invent a cutoff to fill the gap.

**Hand-off:** “The raw match is recorded, but its current autosomal segment
profile and scoring filter remain unresolved through this connection. Next,
obtain the corresponding bounded pair-evidence report.”

## Related

- [SKILL.md](../SKILL.md) — concise research hand-off
- [Connector guide](../references/kinprove-connector-tools.md) — current tool scopes
- [Endogamy](../references/endogamy.md) — settings and native sensitivity work
- [Triangulation](../references/triangulation.md) — limits of interval evidence
