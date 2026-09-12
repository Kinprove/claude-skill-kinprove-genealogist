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

**Fictional study question:** do Ada, Boris, and their niece Celia connect to
the family of Ezra and Mina through their mother/grandmother Irma? The user
has authorized inspection plus copied POI studies for native generation and
cohort comparisons. Source-tree changes are outside that authorization.

### The supplied family and evidence fixture

Ada and Boris are full siblings. Their full sibling Dorian is Celia's father;
Irma is the mother of Ada, Boris, and Dorian. Ada, Boris, Celia, and Irma are
tested; Dorian is not. The baseline POIs are **Ada, Boris, and Celia**.

The documented counterpart family has these paths:

| Path from Ezra and Mina | Tested people |
|---|---|
| Levi → Lena → Mara | Mara |
| Nora → Noah → Niko | Niko |
| Oren → Olga → Owen | Owen |
| Mara and Niko are Dana's parents | Dana, reachable through both Levi and Nora |

Dana is one tester with two ancestral paths. Mara, Niko, and Dana form a
parent/child group, not three independent copies of the same evidence. Even
the other branch is not certified independent merely by its different label.
Mara has two provider kits; the supplied pair evidence below names one chosen
kit per person. Never add her second kit as another participant or sum both.

Fixture metadata specifies a source profile of **`default`**, POI
**`endogamy_mode: true`**, and a verified **7 cM hypothesis-evidence floor**
for this fictional snapshot. The floor is supplied separately; it is not
inferred from either setting. Detector/import/triangulation thresholds are
unspecified. In a live study, unavailable effective metadata stays unknown.

The following are complete autosomal length summaries for the named fixture
pairs after that floor, from the same snapshot and selected pairs. They are
input facts for reasoning, not a fabricated MCP response or a calculated
native score. All fixture segment coordinates use GRCh37.

| Pair | Segment lengths, cM | Total / count / largest | Relevant interval evidence |
|---|---|---|---|
| Irma–Mara | 32, 14, 9 | 55 / 3 / 32 | The 32 cM tract is on chr3, 40–70 Mb |
| Ada–Mara | 14, 9 | 23 / 2 / 14 | The 14 cM tract lies inside it, chr3, 50–64 Mb |
| Boris–Mara | 24, 9 | 33 / 2 / 24 | The 24 cM tract overlaps it, chr3, 44–68 Mb |
| Celia–Mara | 9, 8 | 17 / 2 / 9 | Both are elsewhere; this completed comparison has no retained chr3 tract in that interval |
| Ada–Niko | nineteen segments of 8 cM | 152 / 19 / 8 | All-short sharing despite the larger sum |
| Celia–Owen | Uncomputed | Unknown / unknown / unknown | No measurement; do not replace with zero |

Coordinates do not determine cM by a constant Mb-to-cM conversion. The table
supplies both; it does not assert phase or transmission from Ezra and Mina.
Other family pairs must be read in a real investigation, not assumed to match
these examples.

### Connector sequence and decisions

1. Read the established source project, kit inventory, POI study, and
   participants. Use `get_ancestors` / `get_relationship` and available tree
   topology to verify the sibling/niece structure and all counterpart paths.
   Inventory tested relatives beyond Ada's immediate matches, including Irma,
   Boris, Celia, and Dana. State any traversal or unlinked-kit gaps.
2. Obtain each material pair's source-appropriate evidence and settings.
   Reconcile native selected pairs and filtering where possible; otherwise
   report the mismatch or unknown. A current raw total, an older DNA-check
   maximum, and an imported overlap are not one coherent segment profile.
3. In the authorized working study, use all three related POIs, preserving
   the known Ada–Boris–Dorian–Celia relationships. Read actual participants
   rather than assuming automatic selection included every counterpart.
   Run `check_hypothesis_readiness` and authorized `generate_hypotheses`.
4. Read each POI's `list_hypotheses` and `get_hypothesis_detail`, then
   `list_composite_hypotheses`. Inspect components, applicable fits, actual
   MRCA paths, and the native treatment of Dana's multiple paths. Do not
   manually combine path expectations or add a long-segment bonus.
5. Check the joint topology. For example, a candidate Ezra/Mina → hypothetical
   child → Irma → Ada/Boris places the siblings three generations from the
   couple, with Celia four generations away through Dorian. This is only a
   hypothesis for Irma's origin. A composite that gives Ada and Boris
   unrelated parents, or attaches Celia as their sibling, violates the known
   family even if ranked first. If topology inspection requires
   `materialize_composite_hypothesis`, do that only in an authorized copy and
   inspect `get_ghost_tree`; leave the source family intact.
6. Investigate the Irma–Mara long tract and the shorter overlapping Ada/Boris
   tracts as a transmission lead. Overlap alone does not prove they are the
   same inherited copy. Celia's lack of a retained tract can reflect
   non-inheritance or filtering; it does not remove her branch. Do not sum
   Irma's, Ada's, and Boris's overlapping copies as independent support.
   Ada–Niko's 152 cM short-only sum neither proves a close relationship nor
   quantifies endogamy. A native `good` fit would still require this caveat.
7. Preserve the full baseline, then `duplicate_poi_project` for each
   authorized sensitivity comparison. Compare Mara's, Niko's, and Owen's
   descendant selections; compare Irma as an older focal tester with the
   younger POIs; inspect a copy with/without Dana. Read back each cohort and
   native run. These are overlapping selections, not independent panels.
   Compare placements and supporting fits, not cross-cohort probabilities.
8. If a stricter local filter is requested, inspect the live schema. Without
   a POI-local filter, report that capability limit; do not change the source
   profile, invent a parameter, or present a manually filtered sum as a new
   native score. The existing POI flag has not raised the fixture's floor.

**Without the connector:** the supplied fixture supports the same qualitative
investigation priorities and uncertainty. It cannot supply native ranks,
weights, or composite validation; report those as unavailable.

**Short hand-off:** “Investigate the Irma–Mara connection first: the longer
tract and the siblings' overlapping tracts offer a transmission lead. They
have not been assigned to Ezra and Mina, and Celia–Owen is unmeasured. Next,
verify Irma's proposed parent link and trace that tract across the family.”

### What this example does and does not establish

The 7 cM floor coexists with POI endogamy. The larger short-only total is not
proof of closeness. Celia's observed locus absence and her uncomputed pair
remain different states. Dana retains both paths without becoming two
independent testers. No relationship or score is invented from these numbers;
the next action follows from the evidence's research value.

## Workflow 5 — An empty region read and unknown filters

**Fictional input:** Tess and Uri have a current `get_kit_matches` row totaling
96 cM across 12 segments. `get_segment_region_overlaps` returns no imported
rows in the requested interval. An older DNA-check run reports a 16 cM
largest autosomal segment. The connected schema exposes no complete current
raw pair distribution or effective hypothesis-filter metadata.

Do not combine 96 cM and the old 16 cM maximum into a current autosomal
profile: the raw-kit total's chromosome scope and the snapshots differ.
The empty imported read says nothing about raw overlap there. Neither it nor
a POI endogamy flag supplies a cutoff, a measured zero, or parental origin.
Read available native fits and known paths, preserving these limitations.
If a bounded native pair export is available within authorization, use it;
otherwise leave lengths, effective filters, and selected scoring pair unknown.

**Short hand-off:** “A raw match is recorded, but this connector cannot yet
establish its current autosomal length profile or effective scoring filter.
The empty imported interval is not evidence against the branch. Next, obtain
a current, bounded pair-evidence export before interpreting the score.”

## Related

- [SKILL.md](../SKILL.md) — concise research hand-off
- [Connector guide](../references/kinprove-connector-tools.md) — current tool scopes
- [Endogamy](../references/endogamy.md) — settings and native sensitivity work
- [Triangulation](../references/triangulation.md) — limits of interval evidence
