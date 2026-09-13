# Kinprove Genealogist Claude Skill

A Claude Skill that turns Claude into a Kinprove-aware genealogy research partner.

## What it does

- Helps with DNA matches, triangulation, and family hypotheses in Kinprove projects
- Explains native calculations, selected kits, effective filters, and the limits of stored results
- Ships Kinprove workflows in `examples/`, including related-POI family comparisons and missing-evidence handling
- When paired with the Kinprove MCP connector, reads your actual matches/projects/trees instead of asking you to paste data
- Works from Kinprove exports and study notes without the connector, while identifying unavailable native calculations

The general DNA research methods, family-comparison recipe, and fictional
teaching case are maintained in [DNA Research Recipes](https://github.com/Kinprove/dna-research-recipes/blob/253195738dc9d2d0722b1520d75a37f150ae9a3e/dna-research-recipes/recipes/ai-for-dna-research.md#compare-families-across-branches-and-generations).
That catalog works across platforms and requires no Kinprove account. This
skill explains how to apply the method through Kinprove; the catalog does not
need to be installed as another skill.

## Install

Claude.ai uploads a custom Skill as a **single ZIP archive containing the
skill folder** — not as loose files.

1. Download or clone this repository.
2. Zip the `kinprove-genealogist/` folder so the archive contains the folder
   with `SKILL.md`, `references/`, and `examples/` inside it. For example:
   `zip -r kinprove-genealogist.zip kinprove-genealogist`.
3. In Claude (claude.ai or Claude Desktop), make sure **Code execution and
   file creation** is enabled under **Settings → Capabilities** — Skills need
   it to run.
4. Go to **Settings → Customize → Skills**, choose **Create skill**, and
   upload `kinprove-genealogist.zip`. The Skill then appears in your Skills
   list and can be toggled on or off.
5. (Recommended) Also connect the **Kinprove** connector via **Settings →
   Connectors** so the Skill can use your live data.

> Claude's settings menus change over time — if a label above does not match,
> follow the equivalent path. See Claude Help: "Use Skills in Claude".

## Pair with the Kinprove MCP Connector

The Skill works without the connector, but the connector unlocks reading your real Kinprove projects:

- **Anthropic Connectors Directory** (coming soon) — search for "Kinprove"
- **Manual setup** — Settings → Connectors → Add custom connector → `https://api.kinprove.io/mcp/v1` with an API key from kinprove.io Settings → API Keys

## License

MIT for the Skill's original code and documentation (see `LICENSE`).

The relationship-to-cM range tables in `references/cm-ranges.md` and
`references/mrca-estimation.md` are adapted from the Shared cM Project
v4.0 by Blaine Bettinger, used under [Creative Commons Attribution 4.0
International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
That third-party content keeps its CC BY 4.0 terms — see `NOTICE`.

## Source

Maintained at <https://github.com/kinprove/claude-skill-kinprove-genealogist>. For Kinprove product support, visit <https://kinprove.io/support>.
