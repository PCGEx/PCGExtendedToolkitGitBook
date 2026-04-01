---
description: >-
  Filters vertices based on the dot product (angle) between their connected
  edges.
icon: circle-dashed
---

# Vtx Filter : Edge Angle

### Overview

This vertex filter computes the angle between edges connected to a vertex by calculating the dot product of their directions. This is primarily useful for binary nodes (vertices with exactly two connections) where you want to filter based on whether the vertex represents a sharp turn, gentle curve, or straight line. Fallback behaviors handle edge cases like leaf nodes (one connection) or complex intersections (more than two connections).

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Edge Detection**: Identifies the edges connected to each vertex.
2. **Fallback Check**: For leaves (1 edge) or non-binary nodes (3+ edges), returns the configured fallback result.
3. **Angle Computation**: For binary nodes, computes the dot product between the two edge directions.
4. **Comparison**: Compares the dot product against the threshold using the configured comparison settings.
5. **Result**: Returns pass/fail based on the comparison and invert setting.

**Usage Notes**

* **Binary Nodes**: This filter is most meaningful for vertices with exactly two connections.
* **Dot Product Range**: Values range from -1 (opposite directions, 180°) to 1 (same direction, 0°).
* **Fallback Handling**: Configure how leaves and complex nodes are treated to avoid unexpected results.

### Behavior

```
Edge Angle Examples (for binary nodes):

Straight Line (dot ≈ -1):
  ←──[V]──→  Two edges pointing opposite directions

Sharp Corner (dot ≈ 0):
  ↑
  [V]──→  Two edges at 90° angle

Hairpin Turn (dot ≈ 1):
  →──[V]
     ↓    Two edges pointing similar direction
```

### Settings

<details>

<summary><strong>Leaves Fallback</strong> <code>EPCGExFilterFallback</code></summary>

What result to return for leaf nodes (vertices with only one edge).

| Option   | Description                |
| -------- | -------------------------- |
| **Pass** | Leaf nodes pass the filter |
| **Fail** | Leaf nodes fail the filter |

Default: `Fail`

</details>

<details>

<summary><strong>Non-Binary Fallback</strong> <code>EPCGExFilterFallback</code></summary>

What result to return for non-binary nodes (vertices with more than two edges).

| Option   | Description                      |
| -------- | -------------------------------- |
| **Pass** | Non-binary nodes pass the filter |
| **Fail** | Non-binary nodes fail the filter |

Default: `Fail`

</details>

<details>

<summary><strong>Dot Comparison Details</strong> <code>FPCGExDotComparisonDetails</code></summary>

Settings for the dot product comparison, including threshold value and comparison operator.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Whether to invert the filter result. Note that this also inverts fallback results.

Default: `false`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Nodes/PCGExNodeEdgeAngleFilter.h)
