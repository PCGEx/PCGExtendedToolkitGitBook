---
description: Partition clusters by extracting largest axis-aligned boxes.
icon: function
---

# Decomp : Max Boxes

### Overview

Voxelizes the cluster and iteratively extracts the largest axis-aligned box of occupied voxels. Each extracted box becomes a cell. Boxes exceeding the maximum cell size are subdivided. Adjacent cells that form a perfect larger box are merged. The result is a set of compact, solid-filled rectangular regions.

### How It Works

1. **Voxelization**: The cluster is mapped onto a 3D occupancy grid.
2. **Largest Box Extraction**: For each Z-range, a 2D largest-rectangle-in-histogram algorithm finds the biggest box of occupied voxels. The highest-scoring box is extracted and its voxels are marked as consumed.
3. **Scoring**: Boxes are scored by `Volume * Compactness^(Balance * 2)`, where compactness is the ratio of the smallest to largest dimension. Higher Balance values prefer cube-like shapes over elongated strips.
4. **Subdivision**: Extracted boxes larger than Max Cell Size are subdivided into smaller chunks.
5. **Merging**: Adjacent cells whose combined bounding box is exactly filled (no gaps) are merged back together.
6. **Mapping**: Voxel cell IDs are mapped back to cluster node indices. Cells with fewer occupied voxels than the minimum are discarded.

{% hint style="info" %}
This is a simplified version of the [decomp-max-boxes-extended.md](decomp-max-boxes-extended.md "mention") mode.
{% endhint %}

### Settings

<details>

<summary><strong>Transform Space</strong> <code>EPCGExDecompTransformSpace</code></summary>

How to orient the voxel grid relative to the cluster.

| Option       | Description                                     |
| ------------ | ----------------------------------------------- |
| **World**    | Use world-space axes                            |
| **Best Fit** | Orient the grid to the cluster's principal axes |
| **Custom**   | Use a user-specified transform                  |

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

Manual voxel size.

Default: `(100, 100, 100)`

📋 _Visible when Voxel Size Mode = Manual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Cell Size</strong> <code>FVector</code></summary>

Maximum dimensions for output cells in world units. Extracted boxes larger than this are subdivided.

Default: `(500, 500, 500)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Voxels Per Cell</strong> <code>int32</code></summary>

Minimum occupied voxels per cell. Cells below this threshold are discarded.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Balance</strong> <code>double</code></summary>

Penalizes elongated strips in favor of compact, cube-like boxes. At 0, the algorithm purely maximizes volume (largest box first). Higher values prefer square-like shapes.

Default: `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompMaxBoxes.h)
