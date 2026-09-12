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

Constituent pairwise segments in a computed triad cover that eligible triad,
not every segment for the pair. Provider-imported triads and imported pair
rows also have distinct availability. An empty triad result does not mean
there are no pairwise segments or no genealogical relationship.

Native hypothesis triangulation support can be inspected with
`get_hypothesis_detail`, including its optional `triangulation_trace`. That
trace reproduces aggregate contributions and deduplication using available
evidence; aggregate agreement does not certify historical provenance.
`cross_validation_score` reuses pairwise cM and is not independent validation.
Read native components rather than applying an extra overlap or length bonus.

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
