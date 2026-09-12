# X-chromosome inheritance

Keep X-DNA separate from autosomal DNA. Under the usual sex-chromosome
inheritance model, a male inherits his X from his mother; a female inherits
one X from each parent. A father transmits his X to daughters, not sons.

## Trace the particular segment's possible paths

A male's X ancestry starts through his mother. At each generation, exclude a
father-to-son transmission for that X path; do not exclude every paternal
connection in the wider pedigree. A female can inherit X through her father,
whose own X came from his mother.

A credible shared X segment can constrain its transmitting paths. It does
not by itself prove that the proposed couple transmitted it: relatives can
share through another path. Coordinate overlap is not parental phasing.
Unknown sex, missing ancestry, or no demonstrated path in an incomplete tree
is not the same as a biologically impossible recorded transmission.

Absence of a shared X segment does not generally exclude a genealogical
relationship. The relative may not have inherited it, the source may lack
usable X data, or the result may fall outside the relevant filter. Do not
infer X-body data availability from a provider's file header alone.

## Kinprove platform behavior

Read the current connector schemas and evidence source:

- DNA-check `observed_autosomal_cm` / `autosomal_segments` and
  `observed_x_cm` / `x_segments` are separate. A general raw-kit `total_cm`
  is not automatically autosomal-only; see the [connector guide](kinprove-connector-tools.md).
- `get_xdna_analysis` provides native X analysis through its declared actions.
  `get_kit_triangulations` can return X triads with method, phasing, parental
  origin, and recorded-tree X-path metadata.
- `get_hypothesis_detail.participant_fits` can report `observed_x_cm`,
  `x_path_valid`, and `x_blockage_reason`. A null verdict can mean the check
  was not evaluated or the path could not be established; read the reason.
- The native X-triangulation scoring component is additive-only. Other
  X-path checks have separate meanings; inspect actual native components
  rather than treating all X results as a universal bonus or exclusion.

Detector, import, triangulation, and scoring significance thresholds are
separate settings. Establish the applicable effective value where exposed.
A POI `endogamy_mode` flag does not establish an increased X threshold. If a
cutoff is unavailable, say so instead of supplying a universal 10/15 cM rule.

## Hand-off

State whether the observed X evidence is consistent with the particular
recorded path, what remains unknown, and which relationship or data check
would resolve it. Do not turn a missing/unknown X result into an exclusion or
an unphased overlap into a named ancestral assignment.

## Related

- [Triangulation](triangulation.md) — unphased X and autosomal interval evidence
- [Endogamy](endogamy.md) — separate filtering and POI scoring scopes
- [Connector guide](kinprove-connector-tools.md) — fields and source limitations
