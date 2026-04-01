---
description: >-
  Configures how clusters are built and output, including edge positioning, size
  filtering, and caching options.
icon: sliders-simple
---

# Graph Builder Details

### Overview

This settings block controls the final output stage of cluster-building operations. It determines edge point positioning, basic edge solidification for visualization, cluster size filtering, and optional caching for downstream operations. These settings are shared across nearly all cluster-generating nodes.

### How It Works

1. **Build Graph**: Cluster structure is created from vertices and edges
2. **Position Edges**: Edge points are placed along each edge at the specified position
3. **Apply Solidification**: Optionally set edge bounds for visualization
4. **Filter by Size**: Remove clusters that are too small or too large
5. **Cache Results**: Optionally pre-compute data for downstream nodes

### Behavior

```
Edge Position Example:

Vtx A ●─────────────────● Vtx B

EdgePosition = 0.0:  Edge point at A
EdgePosition = 0.5:  Edge point at midpoint (default)
EdgePosition = 1.0:  Edge point at B

Cluster Size Filtering:
┌────────────────────────────────────────┐
│ Cluster with 2 vtx, 1 edge  → Removed  │
│ Cluster with 5 vtx, 6 edges → Kept     │
│ Cluster with 1000 vtx       → Removed  │
└────────────────────────────────────────┘
(with Min Vtx=3, Max Vtx=500)
```

### Settings

#### Edge Output

<details>

<summary><strong>Write Edge Position</strong> <code>bool</code></summary>

When enabled, edge points are positioned along their edges according to the Edge Position value.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Position</strong> <code>double</code></summary>

Interpolation position along each edge for the edge point. 0 = start vertex, 0.5 = midpoint, 1 = end vertex.

Default: `0.5`

📋 _Visible when Write Edge Position = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Solidification</strong> <code>FPCGExBasicEdgeSolidificationDetails</code></summary>

Basic edge solidification settings for visualization. Sets edge bounds along a specified axis based on endpoint radii. For full control, use the Edge Properties node instead.

| Sub-Setting             | Description                                             |
| ----------------------- | ------------------------------------------------------- |
| **Solidification Axis** | Which axis to solidify along (None, X, Y, Z)            |
| **Radius Type**         | How to calculate edge radius (Lerp, Min, Max, Constant) |
| **Radius Constant**     | Fixed radius when using Constant type                   |
| **Radius Scale**        | Multiplier applied to calculated radius                 |

📋 _Visible when Write Edge Position = true_

</details>

#### Size Filtering

<details>

<summary><strong>Remove Small Clusters</strong> <code>bool</code></summary>

When enabled, discards clusters below the minimum vertex and edge counts.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Vtx Count</strong> <code>int32</code></summary>

Minimum number of vertices required to keep a cluster.

Default: `3`

📋 _Visible when Remove Small Clusters = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Edge Count</strong> <code>int32</code></summary>

Minimum number of edges required to keep a cluster.

Default: `3`

📋 _Visible when Remove Small Clusters = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remove Big Clusters</strong> <code>bool</code></summary>

When enabled, discards clusters above the maximum vertex and edge counts.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Vtx Count / Max Edge Count</strong> <code>int32</code></summary>

Maximum vertex and edge counts allowed. Clusters exceeding these are discarded.

Default: `500`

📋 _Visible when Remove Big Clusters = true_

⚡ PCG Overridable

</details>

#### Advanced Options

<details>

<summary><strong>Refresh Edge Seed</strong> <code>bool</code></summary>

When enabled, regenerates random seeds for edge points.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Build And Cache Clusters</strong> <code>EPCGExOptionState</code></summary>

Controls whether cluster data is cached for reuse by downstream nodes.

Default: `Default`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Pre-Build Face Enumerator</strong> <code>bool</code></summary>

When enabled, pre-computes face enumeration data for cell-finding operations. Downstream nodes with matching projection settings will reuse this cache.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Edge Length</strong> <code>bool</code></summary>

When enabled, writes each edge's length to an attribute.

Default: `false` · Attribute: `EdgeLength`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExGraphs-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExGraphs/Public/Graphs/PCGExGraphDetails.h)
