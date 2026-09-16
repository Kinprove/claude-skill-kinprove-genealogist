# Triangulation and interval evidence

Distinguish a shared-match network, an all-three pairwise interval overlap,
and a phased shared haplotype. A returned triangulation row must be interpreted
according to its method; the word alone does not certify identity by descent
or a transmitting ancestral couple.

## Read the evidence contract

- A↔B and A↔C matches alone are ICW (in-common-with), not three-way
  triangulation. Establish the B↔C measurement and comparable intervals too.
- All three pair matches overlapping on the same chromosome and coordinate
  build support further investigation. Unphased overlap does not prove that
  all three share the same parental copy. No universal probability follows
  from the number of overlapping pairs.
- A validated phased shared segment supports common inheritance, but naming
  its source still requires documented descent and consideration of other
  paths, especially in endogamous pedigrees.

## Kinprove output

`get_kit_triangulations` returns stored engine-computed and provider-imported
triads. The live contract describes computed evidence as
`pairwise_interval_overlap`, with `phasing: unphased`; imported evidence has
provider provenance and may have unknown phasing. Read the `evidence` block
and coordinate `build`. Do not upgrade either type to phased evidence.

Pairwise rows attached to a computed triad are a scoped evidence view, not
a complete segment inventory for each pair. Provider-imported triads and imported pair
rows also have distinct availability. An empty triad result does not mean
there are no pairwise segments or no genealogical relationship.

### Computed span and kit scope

In the maintained engine, reference person R supplies the candidate interval
R–A ∩ R–B. One A–B segment must cover the configured fraction of that
candidate (currently 80% of its coordinate length). Accepted pieces can then
be merged. The fraction applies **before merging**: the final stored interval
can have less than 80% A–B coverage, including gaps between accepted pieces.
Its full span is neither the strict intersection of all three pairs nor a
demonstrated continuous shared haplotype. Do not declare a stored triad stale
solely because its boundaries exceed the third pair's displayed interval.
This engine rule does not establish a provider-imported triad's method.

The same trio can produce different intervals for each reference person and
several intervals per chromosome; these are not independent confirmations.
The engine pools kits belonging to one person. Current
`evidence.pairwise_segments` therefore includes raw rows from all kits of each
participant, identified by kit IDs, even though the stored triad names one
kit per person. A view of only that named kit can omit boundary-setting rows.
This pooled triad scope differs from the selected kit pair in a POI fit.

Those pairwise rows are a current read, not a historical Phase 2 input
snapshot. The maintained readers omit `ab_coverage_fraction`,
`merged_by_bp_gap`, and `merged_by_cm_gap`: defaults in older exports were not
measurements of coverage or merging on this path. Do not use their default
`1` / `false` values as proof of full coverage or an unmerged interval.

### Native scoring support

Native hypothesis triangulation support can be inspected with
`get_hypothesis_detail`, including its optional `triangulation_trace`. That
trace reproduces aggregate contributions and deduplication using available
evidence; aggregate agreement does not certify historical provenance. Only a
triangulated row whose triad contains the POI itself scores toward a couple;
a row shared only among participants is recorded as `participant_only` and
describes the participants' own relationship, not the POI's placement. An
autosomal row shorter than the POI project's evidence cutoff
(`effective_scoring_settings.scored_triangulation_min_segment_cm`, the same
value as the pair-evidence floor) never scores; the trace records such an
otherwise eligible row as `below_evidence_cutoff`. X rows keep their own
per-segment product floor described in [X inheritance](x-dna-inheritance.md#kinprove-platform-behavior).
The trace's `support_score` counts merged POI-containing regions only — pairwise
cM and how many related participants share a region are not additional
support. Read native components rather than applying an extra overlap or
length bonus.

## X and endogamy

Keep X separate from autosomal evidence. X-path fields describe what the
recorded tree demonstrates; read blockage/unknown reasons before declaring a
path impossible. The native X-triangulation component is additive-only; this
is distinct from participant-fit X-path checks. Neither supplies phase or
ancestral-couple attribution. See [X inheritance](x-dna-inheritance.md).

Multiple descent paths and population sharing can make a real overlap
ambiguous as to ancestor. Check source/pipeline filters and provenance where
available. A POI endogamy toggle does not establish tighter triangulation or
pair-evidence thresholds. Do not assume a blacklist eliminated every
uninformative region in every stored source.

## Hand-off

State the method and scope of the overlap, the candidate descent connection,
and the record or transmission evidence needed next. Leave the transmitting
couple unassigned when the evidence cannot distinguish competing paths.

## Related

- [Endogamy](endogamy.md) — overlapping paths and research priority
- [Connector guide](kinprove-connector-tools.md) — native triad and pair scopes
- [X inheritance](x-dna-inheritance.md) — X-path constraints
- [MRCA estimation](mrca-estimation.md) — candidate depths, not attribution
