---
description: >-
  Extract a single path from edges clusters, going through every seed points in
  order.
icon: circle-plus
---

# Pathfinding : Plot Edges

### Overview

This node generates a continuous path that visits a sequence of plot (waypoint) points in order, finding the optimal route between consecutive waypoints through the cluster graph. Unlike regular pathfinding which goes from one seed to one goal, this creates a single path connecting multiple waypoints in sequence.

### How It Works

1. **Match Plots to Clusters**: Associates plot collections with their target clusters.
2. **Map Waypoints**: Finds the nearest cluster nodes for each plot point.
3. **Path Between Points**: Uses the search algorithm to find paths between consecutive waypoints.
4. **Concatenate Segments**: Joins all path segments into a single continuous path.
5. **Optionally Close Loop**: Can connect the last point back to the first.

**Usage Notes**

* **Ordered Sequence**: Plot points are visited in their input order.
* **Closed Loop**: Enable to path from the last waypoint back to the first.
* **Failed Segments**: If any segment fails, optionally omit the entire path.
* **Plot Point Insertion**: Original plot points can be inserted into the output path.

### Behavior

```
Plot-Based Path Generation:

Plot Points (A, B, C, D) in order:

Cluster Graph:
    [1]───[2]───[3]───[4]───[5]
     │     │     │     │     │
    [6]───[7]───[8]───[9]───[10]
     │     │     │     │     │
   [11]──[12]──[13]──[14]──[15]

A near [1], B near [5], C near [15], D near [11]

Resulting Path: 1→2→3→4→5→10→15→14→13→12→11
                  (A→B) → (B→C) → (C→D)

Closed Loop: ...→11→6→1 (D back to A)
```

### Inputs

| Pin            | Type   | Description                           |
| -------------- | ------ | ------------------------------------- |
| **Vtx**        | Points | Cluster vertices                      |
| **Edges**      | Points | Cluster edges                         |
| **Plots**      | Points | Ordered waypoint collections to visit |
| **Heuristics** | Params | Heuristic sub-nodes for path scoring  |

### Settings

#### Search Algorithm

<details>

<summary><strong>Search Algorithm</strong> <code>UPCGExSearchInstancedFactory</code></summary>

The pathfinding algorithm to use between waypoints.

Available: A\*, Dijkstra, Bellman-Ford, Bidirectional

⚡ PCG Overridable

</details>

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

How multiple heuristic scores are combined.

Default: `Weighted Average`

</details>

#### Data Matching

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how plot collections are matched to clusters.

| Property               | Description                       |
| ---------------------- | --------------------------------- |
| **Mode**               | Disabled, by tag, or by attribute |
| **Cluster Match Mode** | Match against Vtx or Edges        |
| **Split Unmatched**    | Separate unmatched data           |

</details>

#### Path Composition

<details>

<summary><strong>Add Seed to Path</strong> <code>bool</code></summary>

When enabled, adds the first plot point at the beginning of the path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Add Goal to Path</strong> <code>bool</code></summary>

When enabled, adds the last plot point at the end of the path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Add Plot Points to Path</strong> <code>bool</code></summary>

When enabled, inserts all intermediate plot points into the path at their corresponding positions.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Closed Loop</strong> <code>bool</code></summary>

When enabled, paths from the last waypoint back to the first, creating a closed circuit.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Path Composition</strong> <code>EPCGExPathComposition</code></summary>

Determines what points make up the output paths.

| Option          | Description                      |
| --------------- | -------------------------------- |
| **Vtx**         | Paths contain only vertex points |
| **Edges**       | Paths contain edge midpoints     |
| **Vtx & Edges** | Paths contain both               |

Default: `Vtx`

⚡ PCG Overridable

</details>

#### Node Picking

<details>

<summary><strong>Seed Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how the first plot point finds its cluster node.

//→ See TODO FPCGExNodeSelectionDetails

</details>

<details>

<summary><strong>Goal Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how subsequent plot points find their cluster nodes.

//→ See TODO FPCGExNodeSelectionDetails

</details>

#### Output

<details>

<summary><strong>Omit Complete Path on Failed Plot</strong> <code>bool</code></summary>

When enabled, if any segment between waypoints fails, the entire path is discarded.

Default: `false`

</details>

<details>

<summary><strong>Paths Output Settings</strong> <code>FPCGExPathOutputDetails</code></summary>

Filters output paths by size.

//→ See TODO FPCGExPathOutputDetails

</details>

<details>

<summary><strong>Statistics</strong> <code>FPCGExPathStatistics</code></summary>

Outputs usage statistics to cluster attributes.

</details>

#### Performance

<details>

<summary><strong>Use Octree Search</strong> <code>bool</code></summary>

Uses octree spatial indexing for finding nearest nodes.

Default: `false`

_Advanced setting_

</details>

<details>

<summary><strong>Greedy Queries</strong> <code>bool</code></summary>

When enabled, allocates separate memory for each query allowing parallel execution.

Default: `true`

_Advanced setting_

</details>

#### Warnings

<details>

<summary><strong>Quiet Invalid Plot Warning</strong> <code>bool</code></summary>

Suppresses warning when a plot could not be resolved to valid paths.

Default: `false`

</details>

#### Inherited Settings

→ See Clusters Processor Settings for common cluster processing settings.

### Outputs

| Pin       | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| **Paths** | Points | Continuous paths visiting all waypoints |
| **Vtx**   | Points | Modified vertices                       |
| **Edges** | Points | Modified edges                          |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Elements/PCGExPathfindingPlotEdges.h)
