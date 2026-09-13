---
name: kinprove-genealogist
description: Guides investigations of DNA matches and family-to-family hypotheses in Kinprove projects. Use for Kinprove MCP tools, POI studies, native scoring, and Kinprove exports or study notes. Resolves evidence scopes, selected kits, effective filters, related POIs, and result currency; identifies unavailable native calculations when the connector is absent.
---

# Kinprove Genealogist

Apply genealogy research methods to Kinprove projects and study exports. Keep
documented relationships, research leads, and native model results distinct.
The platform-independent family-comparison method and teaching fixture live
in [DNA Research Recipes](https://github.com/Kinprove/dna-research-recipes/blob/253195738dc9d2d0722b1520d75a37f150ae9a3e/dna-research-recipes/recipes/ai-for-dna-research.md#compare-families-across-branches-and-generations).
This package owns their application through Kinprove. No companion skill
installation is required; if the linked text is inaccessible, use the user's
evidence and these local constraints without inventing the fixture's contents.

## Working method

1. **Use the authorized project and discover the connector's live schemas.**
   A request to inspect a connected project authorizes the bounded reads needed
   for that question. Reuse established scope; do not ask again before each
   tree or DNA read. Resolve an ambiguous project or person before accessing
   unrelated data. Read [the connector guide](references/kinprove-connector-tools.md)
   for tool scopes, pagination, and unavailable capabilities.
2. **Inspect the evidence before interpreting a total.** Identify the people,
   selected kit pair, source, autosomal total/count/largest segment, available
   length distribution, filters, and result currency. Keep X separate. An
   imported-only empty result, an uncomputed pair, and a measured zero are
   different states. Prefer `get_pair_segment_evidence` when exposed, using
   the POI study's scope for scoring comparisons. Missing metadata remains
   unknown; a current evidence read is not a stored scoring snapshot.
3. **For endogamy, read [endogamy.md](references/endogamy.md) first.** Source
   project filtering and POI scoring settings have separate scopes. Turning on
   POI endogamy does not establish a higher hypothesis-evidence floor. Read
   effective settings and their recorded scoring values where exposed; do
   not infer them from a label or substitute the recipe's research filters.
4. **Research families, including their tested relatives.** Discover relevant
   tested descendants and related POIs, record all descent paths, and use the
   native multi-POI workflow. Multiple kits are not multiple people, and
   overlapping branches are not independent evidence. Preserve native
   multipath calculations; inspect whether returned composite placements
   respect the known relationships. Follow the
   [Kinprove family workflow](examples/workflows.md#workflow-4--compare-two-families-under-endogamy).
5. **Use native calculations and inspect their components.** A long segment
   may deserve research attention without earning a native score bonus.
   Check the scorer's actual contract; never add a homemade weight or
   probability. A native rank or `good` fit is conditional on its evidence,
   model, and candidate set. It does not prove a relationship. Published
   [Shared cM ranges](references/cm-ranges.md) help explain overlapping
   possibilities; cite them when using them, without replacing pedigree-aware
   native estimates with a cM-only guess.
6. **Keep segment attribution provisional.** A known cousin, a long segment,
   or an unphased coordinate overlap does not identify the transmitting
   ancestral couple. Use [triangulation evidence](references/triangulation.md)
   and [X-inheritance paths](references/x-dna-inheritance.md) at their stated
   strength. A descendant may inherit a shorter tract or none of a relative's
   tract; that alone does not remove their branch from the pedigree.
7. **Separate reads, experiments, and genealogy edits.** For authorized
   sensitivity experiments, preserve the original POI and work in copies.
   Changing participants or regenerating hypotheses mutates the study;
   changing source people/families asserts genealogy. Honor the user's
   mutation boundaries and existing authorization. A research hypothesis
   does not itself authorize a source-tree edit.

Without a connector, use the user's relevant tree branches, match/segment
exports, and prior hypotheses. State which fields or native calculations are
unavailable; do not fabricate tool output or ask for data already accessible
within the authorized connection.

## References and examples

- [Connector guide](references/kinprove-connector-tools.md) — live discovery,
  native analysis, mutations, and evidence-source limits
- [Endogamy](references/endogamy.md) — effective settings, native support,
  and controlled POI comparisons
- [Workflows](examples/workflows.md) — match grouping, MRCA work, close
  matches, application of the common family recipe, and older-connector fallback
- [MRCA estimation](references/mrca-estimation.md) and
  [cM ranges](references/cm-ranges.md) — relationship possibilities and depths
- [Triangulation](references/triangulation.md) and
  [X inheritance](references/x-dna-inheritance.md) — what segment evidence can establish

## Hand-off

Default to a short conclusion, the material limitation, and one concrete next
action. Put long pair tables, tool traces, and alternative scenarios in an
optional supporting artifact. Preserve the distinction between what the
records establish, what the native model reports, and what remains a lead.
