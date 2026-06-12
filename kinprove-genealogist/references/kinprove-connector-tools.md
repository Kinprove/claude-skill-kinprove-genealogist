# Kinprove MCP Connector — tool catalog

When the Kinprove MCP connector is enabled in Claude, the Kinprove tool family
becomes available. Use these instead of asking the user to paste data they
already have in Kinprove.

Tool names below are the **MCP protocol names** (snake_case). When Claude
exposes them they are fully-qualified as `mcp__kinprove__<name>` — e.g. the
`generate_hypotheses` tool surfaces as `mcp__kinprove__generate_hypotheses`.
The exact set of tools depends on the connector version, so treat this list as
a guide and discover the live names from the tool registry rather than
hard-coding them. All data is scoped to the authenticated user's account.

## Projects

- `list_projects` — show the user's projects
- `get_project` — full detail for one project (people, kits, hypotheses)
- `manage_project` — create / update / delete a project (delete is destructive)

## Individuals & Families

- `list_individuals` — paginated list within a project (supports gender filter)
- `search_individuals` — fuzzy name search (supports "Smith 1850")
- `get_relationship` — explicit relationship between two individuals
- `get_ancestors` — pedigree traversal
- `manage_individual` — create / update / delete people, with parent assignment (destructive on delete)
- `manage_family` — create / update / delete marriages, add children
- `get_eligible_parents` — candidate parents for a given individual

## DNA Kits & Segments

- `list_dna_kits` — kits with provider, status, triangulation counts
- `manage_dna_kit` — get details, reassign, or delete a kit (destructive on delete)
- `list_dna_segment_references` — individuals with imported segments
- `get_dna_segment_detail` — per-individual match breakdown
- `delete_dna_segments` — destructive
- `get_segment_import_history` — past imports by provider
- `get_xdna_analysis` — X-specific analysis (5 actions: analysis, impossible_matches, validations, reachable_ancestors, descendants)
- `get_kit_triangulations` — three-way segment overlaps touching a kit

## DnaCheck (pair-level scoring)

- `run_dna_check` — heavy compute; computes pairwise relationship scoring (prerequisite for the pair tools below)
- `get_dna_check_summary` — high-level results
- `get_dna_check_pairs` — pair-level findings, **ordered by z-score** (deviation from expected sharing), with an optional `individual_id` filter
- `get_dna_check_anomalies` — flagged anomalies
- `review_dna_check_pair` — write back curator notes
- `get_anomalies` — non-pair-specific anomalies
- `manage_pair_overrides` — manual override of pair scoring

## Hypothesis Engine (ghost trees, scoring, paths)

- `check_hypothesis_readiness` — validate prerequisites before generation
- `generate_hypotheses` — runs the hypothesis pipeline; creates ghost branches and auto-clears existing ones (heavy; requires a POI project)
- `list_hypotheses` — generated hypotheses sorted by score
- `get_hypothesis_detail` — single hypothesis evidence paths
- `rescore_hypotheses` — re-run scoring with updated overrides / anchors
- `get_evidence_paths` — paths supporting a hypothesis
- `get_evidence_gaps` — paths missing data
- `get_ghost_tree` — synthetic anchor tree
- `clear_ghost_hypotheses` / `clear_composite_hypothesis` — destructive
- `manage_hypothesis_endpoint` — manage a hypothesis endpoint

## POI Projects (Point-of-Interest research projects)

- `list_poi_projects` / `create_poi_project` / `duplicate_poi_project` / `update_poi_project` / `delete_poi_project`
- `get_poi_project` / `get_poi_project_analysis`
- `get_poi_ancestry` — POI's ancestry trace
- `get_participants` — participant list
- `manage_blocked_anchors` — block / unblock anchor families
- `list_composite_hypotheses` / `materialize_composite_hypothesis` — composite POI work
- `toggle_scenario_ignore` — scenario-level filtering
- `manage_ghost_family` / `manage_ghost_individual` — synthetic tree edits

`create_poi_project` takes a `project_uuid` plus a `pois` array of
`{individual_id, dna_kit_id?}` entries. It does **not** require an anchor
couple or family; `participant_ids` is optional and auto-resolves to all
DNA-connected individuals when omitted.

## Validation

- `get_validation_candidates` — pairs with known relationships + DNA, for ground-truth validation work

## Research

- `get_research_prompts` — canned research prompts curated by the platform

## When to call which

- **Always start with**: `list_projects` to see what is available, then
  `get_project` for the active project to load context.
- **Match analysis**: `list_dna_kits` → `run_dna_check` (prerequisite if not
  yet run) → `get_dna_check_summary` → `get_dna_check_pairs`. Remember
  `get_dna_check_pairs` is ordered by z-score, not by raw shared cM — re-sort
  the returned pairs if the user asked for "top matches by cM".
- **Hypothesis exploration**: `list_poi_projects` (or `create_poi_project`) →
  `check_hypothesis_readiness` → `generate_hypotheses` → `list_hypotheses` →
  `get_hypothesis_detail` → `get_evidence_paths` for ranked, evidence-backed
  leads.
- **Triangulation work**: `get_kit_triangulations` for a specific kit's
  triangulated segments.
- **Tree expansion**: `get_eligible_parents` when the user is filling in
  missing parents and you want Kinprove's candidate suggestions.

## Destructive tools

Tools with `delete_`, `clear_`, or `manage_` in the name can modify or destroy
user data (`manage_*` tools delete on the delete action). **Always confirm with
the user before invoking these.** Summarize what the tool will do, what scope of
data is affected, and wait for explicit approval.

## Authentication

The connector handles auth via OAuth 2.1 — you do not see tokens. The connector
enforces per-user data isolation, so you can only ever see the connected user's
projects. Cross-user access is impossible at the API layer.

## Related

- [SKILL.md](../SKILL.md) — the skill body that references this catalog
- [examples/workflows.md](../examples/workflows.md) — worked workflows that call these tools
- [mrca-estimation.md](mrca-estimation.md) — uses the hypothesis-engine tools
