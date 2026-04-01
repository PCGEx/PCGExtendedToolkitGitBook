---
description: >-
  Filters vertices by comparing the directions of their connected edges against
  a reference direction.
icon: circle-dashed
---

# Vtx Filter : Edge Direction

### Overview

This vertex filter evaluates each vertex by comparing the directions of its connected edges against a reference direction. The adjacency settings control how results from multiple edges are aggregated. This allows you to filter vertices based on whether their connections point in a particular direction — for example, finding vertices where edges point upward or vertices at the end of horizontal segments.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Reference Direction**: Gets the comparison direction from either a constant or a vertex attribute.
2. **Edge Direction Computation**: For each connected edge, computes its direction vector.
3. **Direction Comparison**: Compares each edge direction against the reference using dot product or hash comparison.
4. **Aggregation**: Applies adjacency settings to determine if the vertex passes based on how many edges matched.
5. **Result**: Returns pass/fail based on the aggregated comparison results.

**Usage Notes**

* **Direction Order**: Controls whether edge directions point from the vertex outward or toward the vertex.
* **Dot Comparison**: Uses angular similarity; good for approximate direction matching.
* **Hash Comparison**: Uses exact vector matching; faster but requires precise alignment.
* **Transform Direction**: Optionally transforms the reference direction by the vertex's local transform.

### Behavior

```
Direction Examples (Reference = Up Vector, Adjacency = Any):

Vertex with upward edge:
  ↑
  [V]──→  Has one upward edge → PASS

Vertex with all horizontal edges:
  ←──[V]──→  No upward edges → FAIL
```

### Settings

<details>

<summary><strong>Comparison Quality</strong> <code>EPCGExDirectionCheckMode</code></summary>

The type of comparison to use for direction checking.

| Option   | Description                                       |
| -------- | ------------------------------------------------- |
| **Dot**  | Use dot product comparison for angular similarity |
| **Hash** | Use vector hash comparison for exact matching     |

Default: `Dot`

</details>

<details>

<summary><strong>Adjacency</strong> <code>FPCGExAdjacencySettings</code></summary>

Controls how comparison results from multiple connected edges are aggregated.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Order</strong> <code>EPCGExAdjacencyDirectionOrigin</code></summary>

Controls the orientation of edge direction vectors.

| Option       | Description                                |
| ------------ | ------------------------------------------ |
| **FromNode** | Edge directions point away from the vertex |
| **ToNode**   | Edge directions point toward the vertex    |

Default: `FromNode`

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Where to read the reference direction from.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use a constant direction vector        |
| **Attribute** | Read direction from a vertex attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Direction (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the reference direction from.

📋 _Visible when Compare Against = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Direction</strong> <code>bool</code></summary>

Flip the reference direction vector before comparison.

Default: `false`

📋 _Visible when Compare Against = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>FVector</code></summary>

The constant direction vector to compare edge directions against.

Default: `(0, 0, 1)` (Up Vector)

📋 _Visible when Compare Against = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform Direction</strong> <code>bool</code></summary>

Transform the reference direction by the vertex's local transform before comparison.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Dot Comparison Details</strong> <code>FPCGExDotComparisonDetails</code></summary>

Settings for dot product comparison mode, including threshold and comparison operator.

📋 _Visible when Comparison Quality = Dot_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Hash Comparison Details</strong> <code>FPCGExVectorHashComparisonDetails</code></summary>

Settings for vector hash comparison mode.

📋 _Visible when Comparison Quality = Hash_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Nodes/PCGExNodeEdgeDirectionFilter.h)
