---
description: Partition clusters by recursive convex hull splitting.
icon: function
---

# Decomp : Convex BSP

### Overview

Recursively splits the cluster using PCA-derived planes until each cell is "convex enough." At each step, the algorithm computes the convex hull of the current node set and measures how many nodes are interior (not on the hull). If the ratio of interior nodes exceeds the threshold, the set is bisected along its principal axis. The result is a set of roughly convex groups.

### How It Works

1. **Convexity Check**: Computes the 3D convex hull of the current node set and measures the concavity ratio (fraction of nodes not on the hull).
2. **Split Decision**: If the concavity ratio exceeds Max Concavity Ratio, the set is split. Otherwise it becomes a leaf cell.
3. **PCA Split Plane**: The principal axis of the node positions is found via power iteration on the covariance matrix. The split plane passes through the centroid, perpendicular to the principal axis.
4. **Fallback Planes**: If the principal axis produces an imbalanced split (one side below Min Nodes Per Cell), alternative perpendicular planes are tried.
5. **Recursion**: Both halves are processed recursively until all cells pass the convexity check or limits are reached.

### Settings

<details>

<summary><strong>Max Concavity Ratio</strong> <code>double</code></summary>

Maximum allowed ratio of interior (non-hull) nodes to total nodes. Lower values produce more convex cells but more splits. At 0, every node must be on the convex hull.

Default: `0.01`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Nodes Per Cell</strong> <code>int32</code></summary>

Minimum nodes per cell. Regions with fewer nodes than this become leaf cells without further splitting.

Default: `4`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Cells</strong> <code>int32</code></summary>

Maximum number of cells to produce. Recursion stops once this limit is reached.

Default: `32`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Depth</strong> <code>int32</code></summary>

Maximum recursion depth.

Default: `100`

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompConvexBSP.h)
