---
description: Attempts to find a closed cell of connected edges around seed points.
icon: circle
---

# Find Cells

### Overview

This node finds closed polygonal cells in a cluster graph that contain or are nearest to input seed points. Unlike Find All Cells which discovers every cell, this node targets specific locations based on seed positions. Seeds can optionally expand to claim adjacent cells, and ownership conflicts are resolved based on configurable rules.

### How It Works

1. **Map Seeds**: Associates each seed point with its nearest cluster node or edge.
2. **Find Cells**: Traces closed polygonal cells containing each seed.
3. **Resolve Ownership**: When multiple seeds claim the same cell, applies ownership rules.
4. **Expand Seeds**: Optionally grows seed influence to adjacent cells.
5. **Output Cells**: Builds path data for each valid cell.

**Usage Notes**

* **Seed Picking**: Configure how seeds find their target cluster node (closest edge or node).
* **Ownership Modes**: Seed Order (first wins), Closest (by distance), Best Candidate (by score).
* **Expansion**: Seeds can grow to claim neighboring cells up to a configurable depth.
* **Filtered Seeds**: Optionally output which seeds succeeded or failed to find cells.

### Behavior

```
Seed-Based Cell Detection:

Cluster with Seeds:
    [A]───[B]───[C]
     │ ╲S1╱ │ ╲ ╱ │
    [D]───[E]───[F]
     │ ╱ ╲ │ ╲S2╱ │
    [G]───[H]───[I]

S1 finds cell ABE-D
S2 finds cell EFI-H

Output: Two cell contours, one per seed
Good Seeds: S1, S2
Bad Seeds: (none if both found cells)
```

### Inputs

| Pin       | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| **Vtx**   | Points | Cluster vertices                      |
| **Edges** | Points | Cluster edges                         |
| **Seeds** | Points | Points defining cell search locations |

### Settings

#### Seed Selection

<details>

<summary><strong>Seed Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how seed points find their nearest cluster node.

//→ See TODO FPCGExNodeSelectionDetails

</details>

<details>

<summary><strong>Seed Ownership</strong> <code>EPCGExCellSeedOwnership</code></summary>

How to determine which seed owns a cell when multiple seeds compete.

| Option                | Description                                   |
| --------------------- | --------------------------------------------- |
| **Seed Order**        | First seed in input order wins                |
| **Closest**           | Seed with shortest 3D distance wins           |
| **Closest Projected** | Seed with shortest 2D projected distance wins |
| **Best Candidate**    | Seed with best score (sorted) wins            |

Default: `Seed Order`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Sort direction when using Best Candidate ownership.

Default: `Ascending`

📋 _Visible when Seed Ownership = Best Candidate_

⚡ PCG Overridable

</details>

#### Cell Constraints

<details>

<summary><strong>Constraints</strong> <code>FPCGExCellConstraintsDetails</code></summary>

Filtering rules for which cells to include. Same options as Find All Cells.

⚡ PCG Overridable

</details>

#### Cell Output

<details>

<summary><strong>Artifacts</strong> <code>FPCGExCellArtifactsDetails</code></summary>

Controls what data is output for each cell. Same options as Find All Cells.

⚡ PCG Overridable

</details>

#### Expansion

<details>

<summary><strong>Seed Growth</strong> <code>FPCGExCellGrowthDetails</code></summary>

Expands seed selection to adjacent cells.

| Property   | Description                                             |
| ---------- | ------------------------------------------------------- |
| **Growth** | Number of cell layers to expand (constant or attribute) |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Expansion Attributes</strong> <code>bool</code></summary>

When enabled, writes expansion metadata to output cells.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Pick Count Attribute Name</strong> <code>FName</code></summary>

Attribute name for pick count (how many times a cell was selected).

Default: `PCGEx/PickCount`

📋 _Visible when Write Expansion Attributes is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Depth Attribute Name</strong> <code>FName</code></summary>

Attribute name for expansion depth (0 = direct seed, higher = expanded).

Default: `PCGEx/Depth`

📋 _Visible when Write Expansion Attributes is enabled_

⚡ PCG Overridable

</details>

#### Seed Output

<details>

<summary><strong>Output Filtered Seeds</strong> <code>bool</code></summary>

When enabled, outputs seeds separated by success/failure status.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Seed Mutations</strong> <code>FPCGExCellSeedMutationDetails</code></summary>

How to modify output seed points based on their cells.

| Property                          | Description                              |
| --------------------------------- | ---------------------------------------- |
| **Location**                      | Move seed to cell centroid, center, etc. |
| **Match Cell Bounds**             | Set seed bounds to match cell            |
| **Area/Perimeter/Compactness To** | Write cell metrics to seed properties    |

📋 _Visible when Output Filtered Seeds is enabled_

⚡ PCG Overridable

</details>

#### Projection

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

How the cluster is projected for 2D operations.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

#### Forwarding

<details>

<summary><strong>Seed Attributes to Path Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copies attributes from seed points to output path tags.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Seed Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Forwards attributes from seed points to path points.

//→ See TODO FPCGExForwardDetails

</details>

#### Performance

<details>

<summary><strong>Use Octree Search</strong> <code>bool</code></summary>

Uses octree spatial indexing for proximity queries.

Default: `false`

_Advanced setting_

</details>

#### Inherited Settings

→ See Clusters Processor Settings for common cluster processing settings.

### Outputs

| Pin                | Type   | Description                                  |
| ------------------ | ------ | -------------------------------------------- |
| **Paths**          | Points | Cell contour paths                           |
| **Cell Bounds**    | Points | Oriented bounding boxes (if enabled)         |
| **SeedGenSuccess** | Points | Seeds that found valid cells (if enabled)    |
| **SeedGenFailed**  | Points | Seeds that failed to find cells (if enabled) |
| **Vtx**            | Points | Modified vertices                            |
| **Edges**          | Points | Modified edges                               |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Elements/PCGExPathfindingFindCells.h)
