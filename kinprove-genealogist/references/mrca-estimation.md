# MRCA depth and relationship hypotheses

Shared cM can suggest overlapping relationship possibilities. It does not
supply a unique MRCA or generation depth. Use the documented tree and native
Kinprove expectations before translating a total into a research target.

## Count each person's path separately

Depth is the number of parent-child links from the person to the proposed
common ancestor. The two depths need not be equal.

| Relationship | Depths to the common ancestor | Interpretation |
|---|---|---|
| Full first cousins | 2 and 2 | Shared grandparents |
| First cousins once removed | 2 and 3 | One side has an additional generation |
| Full second cousins | 3 and 3 | Shared great-grandparents |
| Second cousins once removed | 3 and 4 | Asymmetric paths |
| Full third cousins | 4 and 4 | Shared great-great-grandparents |
| Full fourth cousins | 5 and 5 | A possible research depth, not a DNA cutoff |
| Aunt/uncle and niece/nephew | 1 and 2 | Shared parents/grandparents from the respective perspectives |

Half relationships share one ancestor rather than the corresponding couple;
path depths alone do not distinguish full and half relationships. Multiple
paths also need separate representation without manually adding expected cM.
Do not apply a fixed “halve cM per MRCA generation” rule: adding a generation
on one side differs from adding one on both, and observed inheritance varies.

## Use native evidence and published ranges at their proper scope

When connected, inspect an existing POI study with `list_hypotheses` and
`get_hypothesis_detail`. Read native observed/expected sharing, variance,
participant depths, and actual common-ancestor paths. For authorized new
work, use the POI/readiness/generation sequence in the
[connector guide](kinprove-connector-tools.md). Check known relationships
across related POIs instead of estimating each person's placement in isolation.

[Shared cM Project v4 ranges](cm-ranges.md) are useful context for explaining
why several relationships remain possible. Cite the specific range when
using it and distinguish it from a native model's expected interval. Do not
feed a manually filtered endogamy total into a cM-only lookup as corrected
relationship odds. Missing measurements are not zero, and a native `good`
fit is not proof of the candidate couple.

## Hand-off

Give the supported candidate relationship/path depths, the material unresolved
link or alternative path, and the next record or comparison needed. Record
convergence and segment evidence support a hypothesis at their actual
strength; neither an identical name nor an unphased overlap confirms an MRCA.

## Related

- [cM ranges](cm-ranges.md) — attributed empirical ranges
- [Endogamy](endogamy.md) — native multipath expectations and segment profiles
- [Triangulation](triangulation.md) — interval evidence and attribution limits
- [Connector guide](kinprove-connector-tools.md) — hypothesis tools and result scope
