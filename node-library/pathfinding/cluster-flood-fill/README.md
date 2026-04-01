---
description: Diffuses seed attributes from vtx onto their neighbors.
icon: circle-plus
---

# Cluster : Flood Fill

### Overview

This node performs a flood fill operation on cluster graphs, spreading attribute values from seed points outward through connected vertices. Starting from selected seed vertices, the diffusion expands along edges following configurable prioritization rules, optionally using heuristics to guide the flow direction and fill controls to limit expansion.

### How It Works

1. **Seed Selection**: Identifies starting vertices using the seed picking configuration (proximity, attribute, or filter-based selection).
2. **Initialize Diffusion**: Creates diffusion instances for each seed point with their initial attribute values.
3. **Expand Outward**: For each iteration, evaluates neighboring vertices and expands the fill based on priority (heuristics or distance).
4. **Apply Controls**: Fill controls (depth limits, thresholds, filters) determine when expansion should stop.
5. **Blend Attributes**: As vertices are claimed, seed attributes are blended onto them according to blending rules.
6. **Write Outputs**: Records diffusion metadata (depth, order, distance, endpoints) and optionally generates paths.

**Usage Notes**

* **Parallel vs Sequential**: Parallel mode expands all active seeds one step before the next iteration, creating balanced growth. Sequential mode fully completes each seed's diffusion before starting the next.
* **Heuristics**: Connect heuristic sub-nodes to guide diffusion direction based on scoring (distance, steepness, attributes, etc.).
* **Fill Controls**: Connect fill control sub-nodes to set stopping conditions (max depth, attribute thresholds, edge filters, etc.).
* **Blend Operations**: Connect blend operation sub-nodes to control how seed attributes are blended onto claimed vertices during diffusion.
* **Conflict Resolution**: When multiple seeds compete for the same vertex, priority determines the winner. With heuristics, higher-scoring paths take precedence.

### Behavior

```
Flood Fill Diffusion:

    Seed Points (S)              Cluster Graph               After Diffusion
         ○                      1───2───3───4                 S₁──S₁──S₁──S₂
         │                      │   │   │   │                 │   │   │   │
         ○                      5───6───7───8      →          S₁──S₁──S₂──S₂
                                │   │   │   │                 │   │   │   │
                                9──10──11──12                 S₁──S₂──S₂──S₂

    (Seeds mapped to vertices)  (Edges define connectivity)  (Attributes spread)

Processing Modes:

Parallel:                       Sequential:
  Step 1: All seeds expand 1     Seed 1: Expand until stopped
  Step 2: All seeds expand 1     Seed 2: Expand until stopped
  Step 3: ...                    Seed 3: ...
  (Balanced growth)              (Complete one at a time)
```

### Inputs

| Pin               | Type   | Description                                                                |
| ----------------- | ------ | -------------------------------------------------------------------------- |
| **Vtx**           | Points | Cluster vertices                                                           |
| **Edges**         | Points | Cluster edges                                                              |
| **Seeds**         | Points | Seed points that initiate diffusion                                        |
| **Heuristics**    | Params | Optional heuristic sub-nodes for guided diffusion                          |
| **Fill Controls** | Params | Optional fill control sub-nodes for stopping conditions                    |
| **Blending**      | Params | Optional blend operation sub-nodes for attribute blending during diffusion |

### Settings

#### Seed Settings

<details>

<summary><strong>Seeds</strong> <code>FPCGExFloodFillSeedPickingDetails</code></summary>

Controls how seed points are matched to cluster vertices and their processing order.

| Property           | Type                       | Description                                                          |
| ------------------ | -------------------------- | -------------------------------------------------------------------- |
| **Seed Picking**   | FPCGExNodeSelectionDetails | How seeds select their starting vertex (nearest, by attribute, etc.) |
| **Ordering**       | EPCGExFloodFillOrder       | Order seeds by Index or Sorting rules                                |
| **Sort Direction** | EPCGExSortDirection        | Ascending or Descending sort order                                   |

</details>

#### Processing Settings

<details>

<summary><strong>Processing</strong> <code>EPCGExFloodFillProcessing</code></summary>

Controls how multiple seed diffusions are executed.

| Option         | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| **Parallel**   | All seeds expand one step per iteration, creating balanced growth patterns |
| **Sequential** | Each seed fully completes its diffusion before the next begins             |

Default: `Parallel`

</details>

<details>

<summary><strong>Diffusion</strong> <code>FPCGExFloodFillFlowDetails</code></summary>

Controls the flow behavior of the diffusion.

| Property             | Type  | Description                                        |
| -------------------- | ----- | -------------------------------------------------- |
| **Priority**         | enum  | How to prioritize expansion (Heuristics, Distance) |
| **Fill Rate Source** | enum  | Read fill rate from Seed or Current point          |
| **Fill Rate Input**  | enum  | Constant or Attribute                              |
| **Fill Rate**        | int32 | Number of vertices to expand per iteration         |

</details>

#### Output Attributes

<details>

<summary><strong>Write Diffusion Depth</strong> <code>bool</code></summary>

When enabled, writes the diffusion depth (number of steps from seed) to each vertex.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Diffusion Depth</strong> <code>FName</code></summary>

Attribute name for diffusion depth (int32). Value is -1 for unvisited vertices.

Default: `DiffusionDepth`

📋 _Visible when Write Diffusion Depth is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Diffusion Order</strong> <code>bool</code></summary>

When enabled, writes the order in which vertices were claimed during diffusion.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Diffusion Order</strong> <code>FName</code></summary>

Attribute name for diffusion order (int32). Value is -1 for unvisited vertices.

Default: `DiffusionOrder`

📋 _Visible when Write Diffusion Order is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Diffusion Distance</strong> <code>bool</code></summary>

When enabled, writes the cumulative edge distance from the seed to each vertex.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Diffusion Distance</strong> <code>FName</code></summary>

Attribute name for diffusion distance (double). Value is 0 for seeds.

Default: `DiffusionDistance`

📋 _Visible when Write Diffusion Distance is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Diffusion Ending</strong> <code>bool</code></summary>

When enabled, marks vertices where diffusion terminated (endpoints with no further expansion).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Diffusion Ending</strong> <code>FName</code></summary>

Attribute name for diffusion ending flag (bool). True for terminal vertices.

Default: `DiffusionEnding`

📋 _Visible when Write Diffusion Ending is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Seed Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Controls which attributes from seed points are forwarded to the vertices they diffuse to.

//→ See TODO FPCGExForwardDetails

</details>

#### Path Output Settings

<details>

<summary><strong>Path Output</strong> <code>EPCGExFloodFillPathOutput</code></summary>

Controls whether and how diffusion paths are output as path data.

| Option         | Description                                                                  |
| -------------- | ---------------------------------------------------------------------------- |
| **None**       | Don't output paths                                                           |
| **Full**       | Output complete paths from seed to each endpoint (creates overlapping paths) |
| **Partitions** | Output path segments, only endpoints overlap                                 |

Default: `None`

</details>

<details>

<summary><strong>├─ Partition Over</strong> <code>EPCGExFloodFillPathPartitions</code></summary>

When using Partitions output, determines how paths are segmented.

| Option     | Description                  |
| ---------- | ---------------------------- |
| **Length** | Partition by path length     |
| **Score**  | Partition by heuristic score |
| **Depth**  | Partition by diffusion depth |

Default: `Length`

📋 _Visible when Path Output = Partitions_

</details>

<details>

<summary><strong>└─ Sorting</strong> <code>EPCGExSortDirection</code></summary>

Sort direction for partitioned paths.

| Option         | Description           |
| -------------- | --------------------- |
| **Ascending**  | Shortest/lowest first |
| **Descending** | Longest/highest first |

Default: `Ascending`

📋 _Visible when Path Output = Partitions_

</details>

<details>

<summary><strong>Seed Attributes to Path Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Converts seed point attributes into tags on output paths.

📋 _Visible when Path Output is not None_

//→ See TODO FPCGExAttributeToTagDetails

</details>

#### Performance

<details>

<summary><strong>Use Octree Search</strong> <code>bool</code></summary>

When enabled, uses octree spatial indexing for finding nearest cluster nodes. May be faster or slower depending on dataset characteristics.

Default: `false`

</details>

### Outputs

| Pin       | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| **Vtx**   | Points | Cluster vertices with diffusion attributes    |
| **Edges** | Points | Cluster edges (pass-through)                  |
| **Paths** | Paths  | Diffusion paths (when Path Output is enabled) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/Elements/PCGExFloodFillClusters.h)
