---
description: Decompose clusters into cells and write a CellID attribute on nodes.
icon: circle-nodes
---

# Cluster : Decomposition

### Overview

Partitions cluster vertices into discrete groups using a selected decomposition algorithm. Each vertex receives an integer cell ID attribute identifying which group it belongs to. The decomposition strategy is selected via an inline sub-node, supporting spatial, topological, and attribute-driven approaches.

### How It Works

1. **Algorithm Selection**: The inline Decomposition sub-node defines the partitioning strategy (BSP, grid, spectral, threshold, etc.).
2. **Cluster Processing**: Each cluster is processed independently. The decomposition operation receives the cluster and optional heuristics data.
3. **Cell Assignment**: The algorithm produces a mapping of node index to cell ID. Each vertex point receives the cell ID as an attribute.
4. **Cell Offset**: Cell IDs are offset per edge collection to ensure uniqueness across multiple edge sets sharing the same vertex data.
5. **Output**: Vertex points are duplicated with cell ID attributes written. Edges are forwarded unchanged.

### Inputs

| Pin                           | Type      | Description                                                                                                                                |
| ----------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Vtx**                       | Points    | Cluster vertex points                                                                                                                      |
| **Edges**                     | Points    | Cluster edge data                                                                                                                          |
| **Heuristics**                | Factories | Optional heuristic sub-nodes for algorithms that need edge scoring (e.g., Spectral). Required when the selected decomposition requests it. |
| **Overrides : Decomposition** | Param     | Optional parameter overrides for the decomposition sub-node                                                                                |

### Settings

#### Decomposition Algorithm

<details>

<summary><strong>Decomposition</strong> <code>UPCGExDecompositionInstancedFactory</code></summary>

The decomposition algorithm to use. Select from the dropdown to configure inline:

| Option                   | Method                                                                           |
| ------------------------ | -------------------------------------------------------------------------------- |
| **BSP Occupancy**        | Voxel grid with recursive axis-aligned BSP splits through empty space            |
| **Convex BSP**           | Recursive PCA bisection until each cell is convex enough                         |
| **Grid Partition**       | Uniform 3D grid quantization with small-cell merging                             |
| **Max Boxes**            | Iterative largest axis-aligned box extraction                                    |
| **Max Boxes (Extended)** | Max Boxes with weights, axis bias, volume preference, and heuristic merge gating |
| **Spectral**             | Graph Laplacian Fiedler vector bisection (topology-aware)                        |
| **Threshold**            | Bins a numeric attribute into value ranges                                       |

</details>

#### Heuristics

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

Scoring mode for combining multiple heuristics when the decomposition uses them.

Default: `Weighted Average`

</details>

#### Output

<details>

<summary><strong>Cell ID Attribute Name</strong> <code>FName</code></summary>

Attribute name for the decomposition cell ID written to each vertex point.

Default: `"CellID"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Cell Count Attribute Name</strong> <code>FName</code></summary>

Optional attribute name for per-node cell population count (how many nodes share this cell). Leave empty to disable.

Default: `None` (disabled)

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| **Vtx**   | Points | Vertex points with cell ID attributes |
| **Edges** | Points | Forwarded cluster edges               |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Elements/PCGExClusterDecomposition.h)
