# Kinprove Genealogist Claude Skill

A Claude Skill that turns Claude into a Kinprove-aware genealogy research partner.

## What it does

- Activates whenever you ask Claude about DNA matches, triangulation, family-tree hypotheses, or genealogy research
- Uses reasoning patterns specific to genetic genealogy (cM ranges, MRCA estimation, X-DNA rules, endogamy detection)
- Ships worked example workflows in `examples/` (grouping a project's matches by ancestor, MRCA estimation, unexpected-close-match triage, endogamy detection)
- When paired with the Kinprove MCP connector, reads your actual matches/projects/trees instead of asking you to paste data
- Works standalone without the connector — give it pasted notes/CSVs and it reasons from those

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

Maintained at <https://github.com/kinprove/claude-skill-kinprove-genealogist> (planned). For Kinprove product support, visit <https://kinprove.io/support>.
