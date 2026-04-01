---
description: Partition clusters using axis-aligned BSP splits through empty space.
icon: function
---

# Decomp : BSP Occupancy

### Overview

Voxelizes the cluster, then recursively splits the voxel grid along axis-aligned planes that pass through empty space. The algorithm prefers to cut through gaps and holes in the cluster's spatial footprint, producing cells that correspond to dense, spatially contiguous groups of nodes. A contiguity post-pass ensures no cell contains disconnected voxel islands.

### How It Works

1. **Voxelization**: The cluster is mapped onto a 3D occupancy grid. Voxel resolution is either inferred from edge lengths or set manually.
2. **Recursive BSP**: The grid is recursively bisected along the axis and position that best separates occupied voxels. Split scoring favors balanced partitions and cuts through empty slices (controlled by Gap Weight).
3. **Leaf Assignment**: Recursion stops when a region reaches Max Depth or contains fewer occupied voxels than Min Voxels Per Cell. All occupied voxels in the region are assigned the same cell ID.
4. **Contiguity Pass**: Each leaf cell is flood-filled. If occupied voxels within a cell are spatially disconnected, they are split into separate cells.
5. **Mapping**: Voxel cell IDs are mapped back to cluster node indices.

### Settings

<details>

<summary><strong>Transform Space</strong> <code>EPCGExDecompTransformSpace</code></summary>

How to orient the voxel grid relative to the cluster.

| Option       | Description                                                       |
| ------------ | ----------------------------------------------------------------- |
| **World**    | Use world-space axes                                              |
| **Best Fit** | Orient the grid to the cluster's principal axes for a tighter fit |
| **Custom**   | Use a user-specified transform                                    |

Default: `World`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom Transform</strong> <code>FTransform</code></summary>

Custom transform for grid alignment.

Default: `Identity`

📋 _Visible when Transform Space = Custom_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Voxel Size Mode</strong> <code>EPCGExDecompVoxelSizeMode</code></summary>

How to determine the voxel grid resolution.

| Option            | Description                                               |
| ----------------- | --------------------------------------------------------- |
| **Edge Inferred** | Automatically derive voxel size from cluster edge lengths |
| **Manual**        | Use a user-specified voxel size                           |

Default: `Edge Inferred`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Voxel Size</strong> <code>FVector</code></summary>

Manual voxel size. Smaller voxels give finer spatial resolution but increase memory and computation.

Default: `(100, 100, 100)`

📋 _Visible when Voxel Size Mode = Manual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Depth</strong> <code>int32</code></summary>

Maximum BSP recursion depth. Higher values allow finer partitioning.

Default: `20`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Voxels Per Cell</strong> <code>int32</code></summary>

Stop splitting if a region has fewer occupied voxels than this. Prevents over-fragmentation of small clusters.

Default: `4`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Gap Weight</strong> <code>double</code></summary>

Weight for the empty-gap bonus in split scoring. Higher values prefer splits through empty space, producing cells that follow the cluster's spatial gaps more aggressively.

Default: `2.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompBSPOccupancy.h)
