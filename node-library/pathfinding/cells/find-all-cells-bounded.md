---
description: >-
  Finds all cluster cells and triages them by spatial bounds relationship
  (Inside/Touching/Outside).
icon: circle
---

# Find All Cells (Bounded)

### Overview

This node extends Find All Cells by classifying discovered cells based on their spatial relationship to input bounds. Cells are categorized as Inside (fully contained), Touching (intersecting), or Outside (no overlap) the bounds region. Results can be output to separate pins or combined with tags.

### How It Works

1. **Find Cells**: Traces all closed polygonal cells in the cluster graph.
2. **Classify Cells**: Tests each cell against the input bounds.
3. **Apply Constraints**: Filters cells based on size, area, and other criteria.
4. **Triage Output**: Routes cells to appropriate outputs based on bounds relationship.

**Usage Notes**

* **Bounds Input**: Connect point data whose bounds define the test region.
* **Triage Flags**: Select which categories (Inside/Touching/Outside) to output.
* **Combined Mode**: Outputs all cells to one pin with classification tags.
* **Separate Mode**: Outputs cells to distinct pins per classification.

### Behavior

```
Cell Classification:

Bounds Region:      ┌─────────────┐
                    │   BOUNDS    │
                    └─────────────┘

Cell A:  ████       Cell B:  ████     Cell C:    ████
        ████               ████               ████
   (Outside)          (Touching)          (Inside)

Output (Separate Mode):
   Paths : Inside   → Cell C
   Paths : Touching → Cell B
   Paths : Outside  → Cell A
```

### Inputs

| Pin        | Type   | Description                                |
| ---------- | ------ | ------------------------------------------ |
| **Vtx**    | Points | Cluster vertices                           |
| **Edges**  | Points | Cluster edges                              |
| **Bounds** | Points | Points defining the classification region  |
| **Holes**  | Points | Optional points marking regions to exclude |

### Settings

#### Output Mode

<details>

<summary><strong>Output Mode</strong> <code>EPCGExCellTriageOutput</code></summary>

How triaged cells are output.

| Option       | Description                                |
| ------------ | ------------------------------------------ |
| **Separate** | Output to separate pins per classification |
| **Combined** | Output all to single pin with tags         |

Default: `Separate`

</details>

<details>

<summary><strong>Triage Flags</strong> <code>uint8</code></summary>

Which cell classifications to include in output. Bitmask of Inside, Touching, Outside.

Default: All enabled

⚡ PCG Overridable

</details>

#### Cell Constraints

<details>

<summary><strong>Constraints</strong> <code>FPCGExCellConstraintsDetails</code></summary>

Filtering rules for which cells to include in output. Same options as Find All Cells.

| Property                        | Description                        |
| ------------------------------- | ---------------------------------- |
| **Rotation Method**             | How to determine cell winding      |
| **Output Winding**              | Clockwise or CounterClockwise      |
| **Aspect Filter**               | Both, Convex only, or Concave only |
| **Size/Area/Perimeter Filters** | Min/max constraints                |
| **Compactness Filter**          | Circularity ratio filter           |

⚡ PCG Overridable

</details>

#### Cell Output

<details>

<summary><strong>Artifacts</strong> <code>FPCGExCellArtifactsDetails</code></summary>

Controls what data is output for each cell. Same options as Find All Cells.

⚡ PCG Overridable

</details>

#### Hole Settings

<details>

<summary><strong>Hole Growth</strong> <code>FPCGExCellGrowthDetails</code></summary>

Expands hole exclusion to adjacent cells.

⚡ PCG Overridable

</details>

#### Projection

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

How the cluster is projected for 2D operations.

//→ See TODO FPCGExGeo2DProjectionDetails

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

#### Separate Mode

| Pin                   | Type   | Description                          |
| --------------------- | ------ | ------------------------------------ |
| **Paths : Inside**    | Points | Cells fully inside bounds            |
| **Paths : Touching**  | Points | Cells intersecting bounds            |
| **Paths : Outside**   | Points | Cells fully outside bounds           |
| **Bounds : Inside**   | Points | OBBs for inside cells (if enabled)   |
| **Bounds : Touching** | Points | OBBs for touching cells (if enabled) |
| **Bounds : Outside**  | Points | OBBs for outside cells (if enabled)  |

#### Combined Mode

| Pin             | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| **Paths**       | Points | All cells with classification tags |
| **Cell Bounds** | Points | All OBBs with classification tags  |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Elements/PCGExPathfindingFindAllCellsBounded.h)
