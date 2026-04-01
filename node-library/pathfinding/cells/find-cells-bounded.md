---
description: >-
  Finds closed cells around seed points and triages them by spatial bounds
  relationship (Inside/Touching/Outside).
icon: circle
---

# Find Cells (Bounded)

### Overview

This node combines seed-based cell finding with bounds-based classification. It finds closed polygonal cells around input seed points and categorizes each cell based on whether it is Inside, Touching, or Outside the provided bounds region. Results can be output to separate pins or combined with classification tags.

### How It Works

1. **Map Seeds**: Associates each seed point with its nearest cluster node.
2. **Find Cells**: Traces closed polygonal cells containing each seed.
3. **Classify Cells**: Tests each cell against the input bounds.
4. **Resolve Ownership**: Handles competing seeds for the same cell.
5. **Triage Output**: Routes cells to appropriate outputs based on bounds relationship.

**Usage Notes**

* **Combined Features**: Merges Find Cells seed targeting with bounds-based classification.
* **Seed Expansion**: Seeds can grow to claim adjacent cells within bounds categories.
* **Dual Filtering**: Cells must pass both constraint rules AND bounds classification.

### Behavior

```
Seeded Cell Classification:

Seeds (S1, S2, S3) targeting cells within and outside bounds:

    ┌─────────────────┐
    │  S1 → Cell A    │  BOUNDS
    │  S2 → Cell B    │
    └─────────────────┘
           S3 → Cell C

Classification:
   Cell A: Inside (S1 found it inside bounds)
   Cell B: Touching (S2's cell intersects bounds)
   Cell C: Outside (S3's cell is outside)
```

### Inputs

| Pin        | Type   | Description                               |
| ---------- | ------ | ----------------------------------------- |
| **Vtx**    | Points | Cluster vertices                          |
| **Edges**  | Points | Cluster edges                             |
| **Seeds**  | Points | Points defining cell search locations     |
| **Bounds** | Points | Points defining the classification region |

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

Which cell classifications to include in output.

Default: All enabled

⚡ PCG Overridable

</details>

#### Seed Selection

<details>

<summary><strong>Seed Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how seed points find their nearest cluster node.

//→ See TODO FPCGExNodeSelectionDetails

</details>

<details>

<summary><strong>Seed Ownership</strong> <code>EPCGExCellSeedOwnership</code></summary>

How to determine which seed owns a cell when multiple seeds compete.

| Option                | Description                         |
| --------------------- | ----------------------------------- |
| **Seed Order**        | First seed in input order wins      |
| **Closest**           | Seed with shortest distance wins    |
| **Closest Projected** | Seed with shortest 2D distance wins |
| **Best Candidate**    | Seed with best score wins           |

Default: `Seed Order`

⚡ PCG Overridable

</details>

#### Cell Settings

<details>

<summary><strong>Constraints</strong> <code>FPCGExCellConstraintsDetails</code></summary>

Filtering rules for which cells to include. Same options as Find All Cells.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Artifacts</strong> <code>FPCGExCellArtifactsDetails</code></summary>

Controls what data is output for each cell.

⚡ PCG Overridable

</details>

#### Expansion

<details>

<summary><strong>Seed Growth</strong> <code>FPCGExCellGrowthDetails</code></summary>

Expands seed selection to adjacent cells.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Expansion Attributes</strong> <code>bool</code></summary>

When enabled, writes pick count and depth to output cells.

Default: `false`

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

📋 _Visible when Output Filtered Seeds is enabled_

⚡ PCG Overridable

</details>

#### Projection & Forwarding

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

How the cluster is projected for 2D operations.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

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

#### Separate Mode

| Pin                   | Type   | Description                  |
| --------------------- | ------ | ---------------------------- |
| **Paths : Inside**    | Points | Cells fully inside bounds    |
| **Paths : Touching**  | Points | Cells intersecting bounds    |
| **Paths : Outside**   | Points | Cells fully outside bounds   |
| **Bounds : Inside**   | Points | OBBs for inside cells        |
| **Bounds : Touching** | Points | OBBs for touching cells      |
| **Bounds : Outside**  | Points | OBBs for outside cells       |
| **SeedGenSuccess**    | Points | Seeds that found valid cells |
| **SeedGenFailed**     | Points | Seeds that failed            |

#### Combined Mode

| Pin             | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| **Paths**       | Points | All cells with classification tags |
| **Cell Bounds** | Points | All OBBs with classification tags  |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Elements/PCGExPathfindingFindCellsBounded.h)
