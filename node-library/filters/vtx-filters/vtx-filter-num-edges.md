---
description: Filters vertices based on the number of edges (neighbors) connected to them.
icon: circle-dashed
---

# Vtx Filter : Num Edges

### Overview

This vertex filter evaluates each vertex by comparing its edge count against a threshold value. This allows you to filter vertices based on their connectivity — for example, finding leaf nodes (1 edge), dead ends, highly connected hubs, or vertices with a specific degree.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Count Edges**: Counts the number of edges connected to each vertex.
2. **Get Threshold**: Retrieves the comparison value from either a constant or vertex attribute.
3. **Comparison**: Compares the edge count against the threshold using the selected comparison operator.
4. **Result**: The vertex passes or fails based on the comparison result.

**Usage Notes**

* **Leaf Detection**: Use `StrictlyEqual` with count 1 to find leaf nodes.
* **Hub Detection**: Use `StrictlyGreater` to find well-connected vertices.
* **Attribute Threshold**: Per-vertex thresholds allow dynamic filtering based on vertex properties.

### Behavior

```
Edge Count Examples (Threshold = 2):

StrictlyEqual (count == 2):
  [V] with 2 edges → PASS
  [V] with 3 edges → FAIL

StrictlyGreater (count > 2):
  [V] with 3 edges → PASS
  [V] with 2 edges → FAIL

EqualOrSmaller (count <= 2):
  [V] with 1 edge  → PASS (leaf node)
  [V] with 2 edges → PASS
  [V] with 3 edges → FAIL
```

### Settings

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when comparing edge count against the threshold.

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

Default: `NearlyEqual`

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant threshold or read from a vertex attribute.

| Option        | Description                             |
| ------------- | --------------------------------------- |
| **Constant**  | Use the same threshold for all vertices |
| **Attribute** | Read threshold from a vertex attribute  |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand A (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the threshold value from.

📋 _Visible when Compare Against = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand A</strong> <code>int32</code></summary>

The constant edge count threshold to compare against.

Default: `0`

📋 _Visible when Compare Against = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance value for approximate comparison modes (NearlyEqual, NearlyNotEqual).

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison = NearlyEqual or NearlyNotEqual_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Nodes/PCGExNodeNeighborsCountFilter.h)
