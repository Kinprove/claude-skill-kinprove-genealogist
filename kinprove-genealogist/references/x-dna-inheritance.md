# X-chromosome inheritance

Keep X-DNA separate from autosomal DNA. The inheritance rules here concern
ordinary 46,XY/46,XX non-PAR X: a male inherits it from his mother; a female
inherits one copy from each parent. A father transmits that copy to daughters,
not sons. Identify pseudoautosomal regions against the source genome build
before applying these paths; PAR can recombine between X and Y.

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
- `get_xdna_analysis` reads the stored native X analysis through its declared
  read actions; `run_xdna_analysis` recomputes and stores it, so it needs the
  user's authorization like any other data change.
  `get_kit_triangulations` can return X triads with method, phasing, parental
  origin, and recorded-tree X-path metadata.
- `get_hypothesis_detail.participant_fits` can report `observed_x_cm`,
  `x_path_valid`, and `x_blockage_reason`. A null verdict can mean the check
  was not evaluated or the path could not be established; read the reason.
- The native X-triangulation scoring component is additive-only. Other
  X-path checks have separate meanings; inspect actual native components
  rather than treating all X results as a universal bonus or exclusion.

### Choose the X question before choosing the view

An X-eligible ancestor and the ancestor who transmitted an observed match are
different questions. For pedigree eligibility, use `get_xdna_analysis` with
`action: reachable_ancestors` and the focal `individual_id`; `descendants`
asks the inverse question. These actions describe paths in the project's
recorded tree, whether or not anyone has a measured X match. A missing parent
there is an endpoint of that tree view, not proof that the person's ancestry
ends. Compare another authorized tree or supplied source when it has more
generations; do not silently join same-named people across trees.

For observed triangulated bands, inspect `get_chromosome_painting` with
`focal_individual_uuid` and `chromosome: "X"` when the live schema exposes it.
Read the band's `evidence` method, phasing, parental origin and X-path fields,
plus `excluded_reason` and the response's `truncated` flag. Its ancestral-couple
attribution does not itself establish a parental copy or a continuous phased
haplotype. A painting is reference-scoped; an empty reference view does not
exclude participant-side or pairwise evidence in another declared surface.

`get_dna_segment_detail` describes imported provider evidence. Absence of X in
that view does not rule out native raw-kit evidence. The `validations` and
`impossible_matches` actions of `get_xdna_analysis` read stored analysis
results; an empty read can mean that the separate analysis has not run. Check
the available evidence and compute state before calling it a measured negative.
Starting or refreshing that analysis remains a separate authorized mutation.

An unattributed band (`attributed: false` or no `couple_ref`) leaves the couple
unresolved; it does not establish the absence of a common ancestor. Returned
`get_relationship` paths can identify candidate routes to investigate. Even if
all returned routes pass through one parent, that is conditional on the tree
and search scope, not measured parent-of-origin. Use an explicit parental-origin
observation or suitable phasing before claiming that the band came from that
parent. A graph-service failure makes the path check unavailable, not unrelated.

Detector, import, triangulation, and scoring significance thresholds are
separate settings. The maintained product counts a stored X segment only
when its individual length is **at least 10.00 cM**, at stored two-decimal
precision. For example, 14 + 9 + 7 cM contributes one segment of 14 cM;
9 + 7 cM contributes no qualifying X evidence. A stored 9.99 cM remains
excluded even if rounded to 10.0 in the display.

Sub-floor rows remain listed as diagnostics, marked `below_x_evidence_floor`;
they do not enter X totals, counts, match membership, or hypothesis evidence.
`get_pair_segment_evidence` exposes `effective_filters.x_segment_floor_cm`
and `distribution.x_below_evidence_floor`. This product rule is separate from
the detector's seed threshold, an imported-triad floor, and an X-path check's
gate on summed evidence. A POI `endogamy_mode` flag leaves the X product
floor and the detector's seed unchanged; it does set the separate imported-triad
floor. Read the connected deployment's actual fields; on an older export
without the cutoff, keep it unknown instead of retroactively claiming that
10 cM was applied. The product cutoff is not a universal biological rule.

## Hand-off

State whether the observed X evidence is consistent with the particular
recorded path, what remains unknown, and which relationship or data check
would resolve it. Do not turn a missing/unknown X result into an exclusion or
an unphased overlap into a named ancestral assignment.

## Related

- [Triangulation](triangulation.md) — unphased X and autosomal interval evidence
- [Endogamy](endogamy.md) — separate filtering and POI scoring scopes
- [Connector guide](kinprove-connector-tools.md) — fields and source limitations
