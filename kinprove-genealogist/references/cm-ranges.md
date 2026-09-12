# Shared cM ranges (Shared cM Project v4.0)

Reference for predicting relationships from shared autosomal centimorgans.
**Always cite the range, not just the average** — DNA inheritance is stochastic.

| Relationship | Average cM | Range cM |
|---|---|---|
| Parent / Child | 3485 | 2376–3720 |
| Full sibling | 2613 | 1613–3488 |
| Half sibling | 1759 | 1160–2436 |
| Grandparent / Grandchild | 1754 | 984–2462 |
| Aunt / Uncle / Niece / Nephew | 1741 | 1201–2282 |
| Great-Aunt / -Uncle / -Niece / -Nephew | 850 | 330–1467 |
| 1st cousin | 866 | 396–1397 |
| 1st cousin once removed (1C1R) | 433 | 102–980 |
| 1st cousin twice removed (1C2R) | 221 | 33–471 |
| 2nd cousin | 229 | 41–592 |
| 2nd cousin once removed (2C1R) | 122 | 14–353 |
| 3rd cousin | 73 | 0–234 |
| 3rd cousin once removed (3C1R) | 48 | 0–192 |
| 4th cousin | 35 | 0–139 |
| 5th cousin | 25 | 0–117 |
| 6th cousin | 18 | 0–71 |

## Notes

- These empirical ranges overlap; membership in a range does not identify a relationship or supply its probability.
- The report sets minimums to zero for distant relationships and calculates those averages among pairs sharing detectable DNA; these are not unconditional inheritance expectations.
- **Endogamy and multiple paths need context.** Inspect the segment profile and documented paths rather than applying a fixed relationship-category correction. A high short-only total neither proves closeness nor excludes a real distant connection.
- **Do not sum per-path expected cM.** Overlapping routes and IBD-counting conventions require native pedigree-aware calculations; multiple kits or inherited copies in related testers are not independent evidence.

## Kinprove platform specifics

Use the native expected distributions and multipath adjustments exposed for
the actual study. Distinguish this published population table from the
scorer's expected interval and filtered observations. A filtered research sum
is not a corrected input for ordinary relationship odds. See the
[connector guide](kinprove-connector-tools.md) for native result scopes.

## When to use this table

1. Explain overlapping relationship possibilities for a supplied autosomal total, citing the relevant ranges.
2. Compare those possibilities with known ages and pedigree paths; do not turn a predicted category into a documented relationship.
3. For a connected POI study, inspect native scoring and its evidence rather than replacing it with a cM-only lookup.

Source: Blaine Bettinger, [Shared cM Project v4.0 report](https://thegeneticgenealogist.com/wp-content/uploads/2020/03/Shared-cM-Project-Version-4.pdf), March 2020, summary tables pp. 52–53 (CC BY 4.0). Selected rows are reproduced above; interpretation notes are adapted for this skill.

## Related

- [mrca-estimation.md](mrca-estimation.md) — turning a cM range into a generation-depth estimate
- [endogamy.md](endogamy.md) — why endogamy inflates these cM totals
- [triangulation.md](triangulation.md) — segment-level evidence that disambiguates overlapping cM bands
