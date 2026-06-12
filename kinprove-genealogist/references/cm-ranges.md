# Shared cM ranges (Shared cM Project v4.0)

Reference for predicting relationships from shared autosomal centimorgans.
**Always cite the range, not just the average** — DNA inheritance is stochastic.

| Relationship | Average cM | Range cM |
|---|---|---|
| Parent / Child | 3485 | 3330–3720 |
| Full sibling | 2613 | 2209–3384 |
| Half sibling | 1759 | 1160–2436 |
| Grandparent / Grandchild | 1766 | 1156–2436 |
| Aunt / Uncle / Niece / Nephew | 1750 | 1201–2282 |
| Great-Aunt / -Uncle / -Niece / -Nephew | 850 | 251–2108 |
| 1st cousin | 866 | 396–1397 |
| 1st cousin once removed (1C1R) | 433 | 102–980 |
| 1st cousin twice removed (1C2R) | 220 | 14–516 |
| 2nd cousin | 229 | 41–592 |
| 2nd cousin once removed (2C1R) | 122 | 14–353 |
| 3rd cousin | 73 | 0–234 |
| 3rd cousin once removed (3C1R) | 48 | 0–192 |
| 4th cousin | 35 | 0–139 |
| 5th cousin | 25 | 0–117 |
| 6th cousin | 21 | 0–96 |

## Notes

- Ranges include outliers — within 1 SD covers ~68% of cases.
- For close relationships (1C and closer), the range narrows; for distant (4C+), the range overlaps with noise.
- **Endogamy inflates cM**. A "1C-shaped" cM total in an endogamous population may be a 2C–3C pair.
- **Multiple paths inflate cM**. If two individuals share multiple recent ancestors, sum the per-path contributions.

## Kinprove platform specifics

Kinprove's `RelationshipCalculator` uses subsumption filtering to handle multi-path scenarios — when one ancestor is "subsumed" by another in the same evidence path, only the shorter path is counted. See the platform's `dna-pipeline/scoring.md` knowledge for details.

For empirical distributions when 5+ matched pairs per degree exist on the user's project, Kinprove uses those over the published averages. See `project_empirical_distributions_limitation.md` in the platform memory.

## When to use this table

1. User pastes a cM number → predict relationship category + cite range
2. Match list analysis → group matches by predicted category, flag outliers
3. Hypothesis testing → check if cM total fits the proposed relationship

Source: Shared cM Project v4.0 by Blaine Bettinger (CC BY 4.0).

## Related

- [mrca-estimation.md](mrca-estimation.md) — turning a cM range into a generation-depth estimate
- [endogamy.md](endogamy.md) — why endogamy inflates these cM totals
- [triangulation.md](triangulation.md) — segment-level evidence that disambiguates overlapping cM bands
