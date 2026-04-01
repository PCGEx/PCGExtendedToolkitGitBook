---
description: Partition clusters by uniform spatial grid.
icon: function
---

# Decomp : Grid Partition

### Overview

Overlays a uniform 3D grid on the cluster's bounding box and assigns each node to the grid cell it falls into. Underpopulated cells (fewer nodes than the minimum) are merged into their nearest neighbor. Simple, predictable, and fast.

### How It Works

1. **Grid Overlay**: A uniform 3D grid with the specified cell size is placed over the cluster's bounding box.
2. **Node Assignment**: Each node's position is quantized to a grid coordinate, assigning it to a cell.
3. **Small Cell Merging**: Cells with fewer nodes than Min Nodes Per Cell are iteratively merged into the nearest neighboring cell by centroid distance.
4. **Compaction**: Cell IDs are re-compacted into a sequential range.

### Settings

<details>

<summary><strong>Cell Size</strong> <code>FVector</code></summary>

Size of each grid cell in world units. Smaller cells produce finer partitioning.

Default: `(100, 100, 100)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Nodes Per Cell</strong> <code>int32</code></summary>

Minimum nodes per cell. Cells below this count are merged into their nearest neighbor.

Default: `1`

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompGridPartition.h)
