---
icon: diagram-project
description: 'Cluster : BFS Depth - Computes breadth-first search depth and distance from seed points to all reachable vertices.'
---

# Cluster : BFS Depth

Computes breadth-first search depth and distance from seed points to all reachable vertices.

## Overview

BFS Depth performs a breadth-first traversal from seed points across a cluster, writing hop count, accumulated edge distance, and/or seed ownership to each reachable vertex. Unreachable vertices retain a value of `-1`. This gives you a per-vertex measure of how "far" each point is from the nearest seed — both topologically (hop count) and spatially (world-space distance along edges).

## How It Works

1. **Seed Matching**: Each seed point is matched to its closest cluster vertex using the configured picking method and distance threshold. Duplicate matches are resolved so each vertex is seeded at most once.
2. **Initialization**: All matched seed vertices are set to depth `0`, distance `0`, and tagged with their seed point index.
3. **BFS Traversal**: The algorithm expands outward from all seeds simultaneously. Each neighbor that hasn't been visited gets depth `current + 1`, accumulated distance along the traversed edge, and inherits the seed owner of the vertex that reached it first.
4. **Attribute Output**: Results are written to the configured attribute names on the vertex point data. Unreachable vertices keep their default value of `-1`.

#### Usage Notes

- **Multiple seeds**: When multiple seeds are used, BFS expands from all of them simultaneously. Each vertex is claimed by whichever seed reaches it first — this creates a natural Voronoi-like partitioning of the cluster.
- **Unreachable vertices**: Disconnected portions of the cluster that no seed can reach will have `-1` for all outputs. This can be used downstream to identify or filter isolated regions.
- **Distance vs Depth**: Depth counts hops (all edges equal), while Distance accumulates actual world-space edge lengths. Two vertices at the same depth can have very different distances if edge lengths vary.

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **Vtx** | Points | Cluster vertices |
| **Edges** | Points | Cluster edges |
| **Seeds** | Points | Seed points used as BFS starting positions |

## Settings

### Node-Specific Settings

<details>
<summary><strong>Seed Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

How seed points select their starting vertex in the cluster. Controls the picking method and maximum search distance.

Default: picking distance `200`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Write Depth</strong> <code>bool</code></summary>

Write the BFS depth — the hop count from the nearest seed. Unreachable vertices keep a value of `-1`.

Default: `true`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Depth</strong> <code>FName</code></summary>

Name of the `int32` attribute to write depth to.

Default: `BFSDepth`

📋 *Visible when Write Depth is enabled*

⚡ PCG Overridable

</details>

<details>
<summary><strong>Write Distance</strong> <code>bool</code></summary>

Write the accumulated edge distance from the nearest seed. Unreachable vertices keep a value of `-1`.

Default: `false`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Distance</strong> <code>FName</code></summary>

Name of the `double` attribute to write distance to.

Default: `BFSDistance`

📋 *Visible when Write Distance is enabled*

⚡ PCG Overridable

</details>

<details>
<summary><strong>Write Seed Index</strong> <code>bool</code></summary>

Write the point index of the closest seed that reached this vertex. Unreachable vertices keep a value of `-1`.

Default: `false`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Seed Index</strong> <code>FName</code></summary>

Name of the `int32` attribute to write seed index to.

Default: `SeedIndex`

📋 *Visible when Write Seed Index is enabled*

⚡ PCG Overridable

</details>

<details>
<summary><strong>Use Octree Search</strong> <code>bool</code></summary>

Whether to use an octree for closest node search. Depending on your dataset, this may be faster or slower.

Default: `false`

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Clusters Processor Settings](../../_staging-concepts/03-clusters.md) for shared cluster processing options.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Vtx** | Points | Cluster vertices with BFS attributes written |
| **Edges** | Points | Cluster edges (forwarded) |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/Elements/PCGExBFSDepth.h)

<!-- VERIFICATION REPORT
Node-Specific Properties: 8 documented
Inherited Properties: Referenced to UPCGExClustersProcessorSettings
Inputs: Vtx, Edges (inherited), Seeds (Labels pin)
Outputs: Vtx, Edges (inherited)
Nested Types: FPCGExNodeSelectionDetails (shared type, referenced)
-->
