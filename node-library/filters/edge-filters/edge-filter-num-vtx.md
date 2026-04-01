---
icon: circle-dashed
---

# Edge Filter : Num Vtx

### Overview

This edge filter evaluates the neighbor count at edge endpoints and compares the result against a threshold. The mode determines how endpoint neighbor counts are combined — using the sum of both endpoints, requiring any endpoint to meet the threshold, or requiring both endpoints to meet it. This allows you to filter edges based on connectivity patterns, such as keeping only edges between well-connected vertices or removing edges that connect to leaf nodes.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Neighbor Counting**: Counts the number of adjacent vertices (neighbors) at both the start and end vertices of each edge.
2. **Mode Application**: Combines the counts based on the selected mode (Sum, Any, or Both).
3. **Threshold Comparison**: Compares the result against the threshold using the selected comparison operator.
4. **Result**: The edge passes or fails based on the comparison result and invert setting.

**Usage Notes**

* **Sum Mode**: Adds neighbor counts from both endpoints before comparing.
* **Any Mode**: Edge passes if either endpoint's neighbor count meets the threshold.
* **Both Mode**: Edge passes only if both endpoints' neighbor counts meet the threshold.
* **Self-Edges**: The neighbor count typically includes the edge itself, so isolated edges have at least 1 neighbor per endpoint.

### Behavior

```
Mode Examples (Threshold = 3, Comparison = EqualOrGreater):

Sum Mode:
  [2 neighbors]---[2 neighbors] → PASS (sum = 4 >= 3)
  [1 neighbor]---[1 neighbor]   → FAIL (sum = 2 < 3)

Any Mode:
  [3 neighbors]---[1 neighbor]  → PASS (3 >= 3)
  [2 neighbors]---[1 neighbor]  → FAIL (neither >= 3)

Both Mode:
  [3 neighbors]---[4 neighbors] → PASS (both >= 3)
  [3 neighbors]---[2 neighbors] → FAIL (one < 3)
```

### Settings

<details>

<summary><strong>Threshold Input</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant threshold or read from an edge attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use the same threshold for all edges  |
| **Attribute** | Read threshold from an edge attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Threshold (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the threshold value from.

📋 _Visible when Threshold Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold</strong> <code>int32</code></summary>

The constant neighbor count threshold to compare against.

Default: `2`

📋 _Visible when Threshold Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mode</strong> <code>EPCGExRefineEdgeThresholdMode</code></summary>

How to combine the neighbor counts from both endpoints before comparison.

| Option   | Description                                    |
| -------- | ---------------------------------------------- |
| **Sum**  | Add both endpoint neighbor counts together     |
| **Any**  | Pass if any endpoint meets the threshold       |
| **Both** | Pass only if both endpoints meet the threshold |

Default: `Sum`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when comparing the neighbor count result against the threshold.

| Option               | Description                            |
| -------------------- | -------------------------------------- |
| **StrictlyGreater**  | Count > Threshold                      |
| **StrictlySmaller**  | Count < Threshold                      |
| **EqualOrGreater**   | Count >= Threshold                     |
| **EqualOrSmaller**   | Count <= Threshold                     |
| **StrictlyEqual**    | Count == Threshold                     |
| **StrictlyNotEqual** | Count != Threshold                     |
| **NearlyEqual**      | Count ≈ Threshold (within tolerance)   |
| **NearlyNotEqual**   | Count !≈ Threshold (outside tolerance) |

Default: `EqualOrGreater`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance value for approximate comparison modes (NearlyEqual, NearlyNotEqual).

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison = NearlyEqual or NearlyNotEqual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the filter result (pass becomes fail and vice versa).

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Edges/PCGExEdgeNeighborsCountFilter.h)
