# Example workflows

Worked end-to-end examples for the Kinprove Genealogist Skill. Each example
shows the **connector path** (tools to call, in order) and the **no-connector
path** (reasoning from pasted data), then a **hand-off summary**.

Connector tool names below are the MCP protocol names (snake_case); Claude
surfaces them fully-qualified as `mcp__kinprove__<name>`. The full catalog and
each tool's contract is in [`references/kinprove-connector-tools.md`](../references/kinprove-connector-tools.md).

Kinprove is **project-scoped**: a "match" is another DNA kit imported into the
same project and compared pairwise — there is no global or external match
database to query. `run_dna_check` computes pairwise IBD across a project's
kits, and `get_dna_check_pairs` returns those pairs. Where a workflow below
says "matches", it means the matching kits within the user's project.

---

## Workflow 1 — "Group the DNA matches in my project by likely common ancestor"

**Connector path:**

1. `list_projects` → pick the project (confirm with the user if more than one).
2. `get_project` → load context (people, kits, hypotheses already present).
3. `list_dna_kits` → identify the user's own kit and the matching kits.
4. `run_dna_check` → **prerequisite** if pairwise scoring has not been computed
   yet (heavy compute — tell the user it may take a while).
5. `get_dna_check_summary` → high-level picture.
6. `get_dna_check_pairs` → pair-level findings. **Important:** this tool returns
   pairs ordered by **z-score** (deviation from the expected sharing for the
   predicted degree), NOT by raw shared cM. To rank a kit's matches by raw
   sharing, pass `individual_id` to scope to that kit, then re-sort the
   returned pairs by shared cM yourself.

Then reason:

- Group the matches into cM bands using [`references/cm-ranges.md`](../references/cm-ranges.md)
  (e.g. 1C-ish 396–1397 cM, 2C-ish 41–592 cM). Bands overlap — say so.
- Cross-reference shared matches: matches that are in-common-with each other
  likely descend from the same ancestral couple. Two-of-three pair matches are
  ICW, not triangulation — see [`references/triangulation.md`](../references/triangulation.md).
- Prefer triangulated segments over raw pair matches when assigning a group to
  an ancestor: `get_kit_triangulations` on the user's kit.
- Watch for endogamy red flags (many matches at every cousin level, inflated
  totals) per [`references/endogamy.md`](../references/endogamy.md). If they
  fire, widen every band by one step and lower confidence.

**No-connector path:** ask the user to paste their match export (one row per
match: name, shared cM, segment count, predicted relationship, and shared-match
list if available). Sort by cM, band with the cM table, and cluster by the
shared-match column. The reasoning is identical — only the data source differs.

**Hand-off summary:** for each group, state the candidate ancestral couple (or
"unidentified — needs tree work"), the cM-band evidence, whether triangulation
supports it, and the single most useful next research step.

---

## Workflow 2 — "Who is the MRCA of these three matches?"

**Connector path (triangulation-first):**

1. `get_kit_triangulations` → confirm the three kits actually triangulate on a
   shared segment. If they only pair-match (ICW), say so — that is weaker
   evidence and the rest of this workflow does not apply cleanly.
2. To run the hypothesis engine you need a **POI project** first:
   - `list_poi_projects` → reuse an existing POI project if one fits, OR
   - `create_poi_project` → it takes a `project_uuid` plus a `pois` array of
     `{individual_id, dna_kit_id?}` entries. It does **not** require an anchor
     couple/family; `participant_ids` is optional and auto-resolves to all
     DNA-connected individuals when omitted.
3. `check_hypothesis_readiness` → validate prerequisites before generating.
4. `generate_hypotheses` → runs the pipeline (heavy; auto-clears prior ghost
   branches for that POI project).
5. `list_hypotheses` → ranked candidate MRCAs.
6. `get_hypothesis_detail` → evidence paths + triangulation support for the
   top candidate.

Then reason: convert the shared-cM band to a generation depth with
[`references/mrca-estimation.md`](../references/mrca-estimation.md). Cite the
range, not a point estimate. The triangulated segment confirms a shared
ancestor *exists* at that locus — it does not name them; tree convergence does.

**No-connector path:** ask for the three trees and the segment overlap
(chromosome + coordinates). Walk each tree back to the depth the cM band
implies and look for a name that appears in all three.

**Hand-off summary:** name the candidate MRCA couple if found; otherwise give
the generation depth and tell the user which of the three trees is shallowest
and should be built back first.

---

## Workflow 3 — "This match shares 1,400 cM but I don't recognize them"

**Connector path:** `list_dna_kits` → `get_dna_segment_detail` for the match's
kit → `get_xdna_analysis` if the user's and the match's sexes make X-evidence
usable.

Then reason: 1,400 cM is in an **overlap zone** — per
[`references/cm-ranges.md`](../references/cm-ranges.md) it is consistent with a
half sibling (1160–2436), grandparent/grandchild (1156–2436), aunt/uncle ↔
niece/nephew (1201–2282), or 1st cousin upper end (396–1397). cM alone cannot
separate these. Discriminators:

- **Age gap** between the two people rules in or out grandparent vs. half-sib
  vs. cousin.
- **X-DNA** narrows the side — [`references/x-dna-inheritance.md`](../references/x-dna-inheritance.md).
  A father transmits no X to sons, so an unexpected close match with X-sharing
  cannot sit on certain paternal lines.
- **An unexpected close match is a flag for an NPE** (non-paternity event,
  misattributed parentage, unknown adoption). Name that possibility gently and
  explicitly — do not bury it.
- If the population is endogamous, even a large total can be more distant than
  it looks — see [`references/endogamy.md`](../references/endogamy.md).

**No-connector path:** ask for the match's predicted relationship from the
testing company, the segment count, the largest segment, and the approximate
ages of both people. Reason from the same discriminators.

**Hand-off summary:** give the shortlist of relationship categories, the
discriminator that would settle it (age, X-path, a documentary record), and a
sensitively-worded note if an NPE is on the table.

---

## Workflow 4 — "My matches all look like cousins at every level"

This is the **endogamy detection** workflow. Follow
[`references/endogamy.md`](../references/endogamy.md).

**Connector path:** `get_dna_check_summary` and `get_dna_check_pairs` to see the
spread. Endogamy shows as a continuous spectrum of "cousin" predictions instead
of clean category clusters, inflated totals, and `get_kit_triangulations`
returning many triangulated loci that do not converge on one MRCA.

Then reason — check the detection signals (two or more firing = likely
endogamous): many "1C2R or closer" predictions the user does not recognise; a
match count that dwarfs comparable non-endogamous trees; tree research already
showing distant cousins on multiple lines; a known endogamous population
background; triangulations scattered without convergence.

If endogamy is confirmed:

- Suggest enabling **endogamy mode** on the POI project (`update_poi_project`).
  It raises cM cutoffs and widens scoring variance per relationship degree.
- Widen every relationship prediction by one category step.
- Pivot research from DNA-alone to documentary records plus multi-line
  corroboration; DNA + endogamy is unreliable past ~4 generations.

**No-connector path:** ask for the match-count distribution by cM band and the
known ethnic background. The signal list is the same.

**Hand-off summary:** name the endogamy diagnosis explicitly, cite which
signals fired, recommend endogamy mode, and reset the user's relationship-
prediction expectations downward.

## Related

- [SKILL.md](../SKILL.md) — the skill body these workflows expand on
- [kinprove-connector-tools.md](../references/kinprove-connector-tools.md) — the tool catalog every workflow draws on
- [mrca-estimation.md](../references/mrca-estimation.md) — the depth-estimation reference used in workflows 1–3
