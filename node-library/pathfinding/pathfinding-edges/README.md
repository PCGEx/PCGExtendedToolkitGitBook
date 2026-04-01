---
description: Extract paths from edges clusters.
icon: circle-plus
---

# Pathfinding : Edges

### Overview

This node finds paths through cluster graphs by navigating from seed points to goal points along edges. It uses configurable search algorithms (A\*, Dijkstra, etc.) guided by heuristics to find optimal or near-optimal paths. The output is a collection of path point data representing the traversed routes through the cluster.

### How It Works

1. **Map Seeds/Goals**: Associates seed and goal points with their nearest cluster nodes.
2. **Pair Queries**: Uses the Goal Picker to determine which goals each seed should path to.
3. **Execute Search**: Runs the search algorithm with connected heuristics to find paths.
4. **Build Paths**: Constructs output path data from the discovered routes.

**Usage Notes**

* **Cluster Required**: Input must be a valid cluster with vertices and edges.
* **Heuristics**: Connect heuristic sub-nodes to guide path selection (distance, steepness, etc.).
* **Goal Picker**: Determines path-goal pairing strategy (all goals, random, attribute-based).
* **Memory vs Speed**: Disable "Greedy Queries" to reduce memory at the cost of speed.

### Behavior

```
Pathfinding Flow:

Seeds (3 points)     Goals (2 points)
    S1 ─────────────────► G1
    S2 ─────────────────► G1
    S3 ─────────────────► G2

Cluster Graph:
    [A]───[B]───[C]
     │     │     │
    [D]───[E]───[F]
     │     │     │
    [G]───[H]───[I]

S1 near [A], G1 near [I]:
    Path: A → B → E → H → I

Output: Path collection with one path per seed-goal pair
```

### Inputs

| Pin            | Type   | Description                          |
| -------------- | ------ | ------------------------------------ |
| **Vtx**        | Points | Cluster vertices                     |
| **Edges**      | Points | Cluster edges                        |
| **Seeds**      | Points | Starting points for pathfinding      |
| **Goals**      | Points | Destination points for pathfinding   |
| **Heuristics** | Params | Heuristic sub-nodes for path scoring |

### Settings

#### Path Composition

<details>

<summary><strong>Goal Picker</strong> <code>UPCGExGoalPicker</code></summary>

Controls how goals are selected for each seed point. This is an instanced sub-node.

Available pickers:

* **All**: Each seed paths to every goal
* **Random**: Each seed paths to a randomly selected goal
* **Attribute**: Goal selection based on matching attributes

⚡ PCG Overridable

</details>

<details>

<summary><strong>Add Seed to Path</strong> <code>bool</code></summary>

When enabled, includes the original seed point at the beginning of each output path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Add Goal to Path</strong> <code>bool</code></summary>

When enabled, includes the original goal point at the end of each output path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Path Composition</strong> <code>EPCGExPathComposition</code></summary>

Determines what points make up the output paths.

| Option          | Description                                    |
| --------------- | ---------------------------------------------- |
| **Vtx**         | Paths contain only vertex points               |
| **Edges**       | Paths contain edge midpoints                   |
| **Vtx & Edges** | Paths contain both vertices and edge midpoints |

Default: `Vtx`

⚡ PCG Overridable

</details>

#### Node Picking

<details>

<summary><strong>Seed Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how seed points find their nearest cluster node.

//→ See TODO FPCGExNodeSelectionDetails

</details>

<details>

<summary><strong>Goal Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how goal points find their nearest cluster node.

//→ See TODO FPCGExNodeSelectionDetails

</details>

#### Search Algorithm

<details>

<summary><strong>Search Algorithm</strong> <code>UPCGExSearchInstancedFactory</code></summary>

The pathfinding algorithm to use. This is an instanced sub-node.

Available algorithms:

* **A\*** - Best-first search using heuristics (fastest for most cases)
* **Dijkstra** - Guaranteed shortest path, ignores heuristics
* **Bellman-Ford** - Handles negative edge weights
* **Bidirectional** - Searches from both ends simultaneously

⚡ PCG Overridable

</details>

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

How multiple heuristic scores are combined when evaluating paths.

| Option               | Description                             |
| -------------------- | --------------------------------------- |
| **Weighted Average** | Averages scores using heuristic weights |
| **Min**              | Uses the lowest score                   |
| **Max**              | Uses the highest score                  |
| **Sum**              | Adds all scores together                |

Default: `Weighted Average`

</details>

#### Tagging & Forwarding

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

<details>

<summary><strong>Goal Attributes to Path Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copies attributes from goal points to output path tags.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Goal Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Forwards attributes from goal points to path points.

//→ See TODO FPCGExForwardDetails

</details>

#### Statistics

<details>

<summary><strong>Statistics</strong> <code>FPCGExPathStatistics</code></summary>

Outputs usage statistics to cluster attributes.

| Property                  | Description                          |
| ------------------------- | ------------------------------------ |
| **Write Point Use Count** | Track how many paths use each vertex |
| **Write Edge Use Count**  | Track how many paths use each edge   |

</details>

#### Path Output

<details>

<summary><strong>Paths Output Settings</strong> <code>FPCGExPathOutputDetails</code></summary>

Filters output paths by size.

//→ See TODO FPCGExPathOutputDetails

</details>

#### Performance

<details>

<summary><strong>Use Octree Search</strong> <code>bool</code></summary>

Uses octree spatial indexing for finding nearest nodes. May be faster or slower depending on data distribution.

Default: `false`

_Advanced setting_

</details>

<details>

<summary><strong>Greedy Queries</strong> <code>bool</code></summary>

When enabled, allocates separate memory for each query allowing parallel execution. Disable to reduce memory usage at the cost of speed.

Default: `true`

_Advanced setting_

</details>

#### Inherited Settings

→ See Clusters Processor Settings for common cluster processing settings.

### Outputs

| Pin          | Type      | Description                                             |
| ------------ | --------- | ------------------------------------------------------- |
| **Paths**    | Points    | Collection of path point data, one per successful route |
| **PathsNum** | Attribute | Number of paths found                                   |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Elements/PCGExPathfindingEdges.h)
