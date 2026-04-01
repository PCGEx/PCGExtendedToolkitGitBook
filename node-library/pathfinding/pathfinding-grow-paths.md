---
description: Grow paths from seeds.
hidden: true
icon: circle-plus
---

# Pathfinding : Grow Paths

{% hint style="danger" %}
This node is deprecated; use [cluster-flood-fill](cluster-flood-fill/ "mention") instead.
{% endhint %}

### Overview

This node generates paths by growing outward from seed points through a cluster graph. Unlike goal-directed pathfinding, growth-based paths extend iteratively based on a growth direction, heuristics, and configurable limits. Seeds can spawn multiple branches that grow in parallel or sequence until they reach maximum iterations, distance, or stop conditions.

### How It Works

1. **Map Seeds**: Associates each seed point with its nearest cluster node.
2. **Initialize Growth**: Sets up growth parameters (iterations, direction, distance).
3. **Iterate Growth**: Extends paths by selecting the best next node based on heuristics.
4. **Check Limits**: Stops growth when reaching max iterations, distance, or stop points.
5. **Output Paths**: Creates path data for each completed growth.

**Usage Notes**

* **Growth Mode**: Parallel grows all seeds simultaneously; Sequence grows one seed completely before the next.
* **Branching**: Seeds can spawn multiple growth branches from their starting node.
* **Dynamic Parameters**: Iterations, direction, and distance can be read from attributes.
* **Stop Points**: Mark vertices where growth should terminate.
* **No-Growth Points**: Mark vertices that paths cannot traverse (but can be seed locations).

### Behavior

```
Growth-Based Path Generation:

Seed S with 2 branches, 3 iterations, direction = Right:

Cluster:
    [A]───[B]───[C]───[D]
     │     │     │     │
    [E]───[S]───[G]───[H]
     │     │     │     │
    [I]───[J]───[K]───[L]

Growth (direction favors rightward):
   Branch 1: S → G → C → D (3 iterations)
   Branch 2: S → G → H → L (3 iterations, different path)

Output: Two paths, both starting from S
```

### Inputs

| Pin            | Type   | Description                            |
| -------------- | ------ | -------------------------------------- |
| **Vtx**        | Points | Cluster vertices                       |
| **Edges**      | Points | Cluster edges                          |
| **Seeds**      | Points | Starting points for growth             |
| **Heuristics** | Params | Heuristic sub-nodes for growth scoring |

### Settings

#### Seed Selection

<details>

<summary><strong>Seed Picking</strong> <code>FPCGExNodeSelectionDetails</code></summary>

Controls how seed points find their nearest cluster node.

//→ See TODO FPCGExNodeSelectionDetails

</details>

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

How multiple heuristic scores are combined.

Default: `Weighted Average`

</details>

#### Growth Mode

<details>

<summary><strong>Growth Mode</strong> <code>EPCGExGrowthIterationMode</code></summary>

Controls how iterative growth is managed.

| Option       | Description                                               |
| ------------ | --------------------------------------------------------- |
| **Parallel** | Grows all seeds one iteration at a time until none remain |
| **Sequence** | Grows each seed to completion before starting the next    |

Default: `Parallel`

</details>

#### Iterations

<details>

<summary><strong>Num Iterations</strong> <code>EPCGExGrowthValueSource</code></summary>

Source for maximum growth iterations per seed.

| Option             | Description                        |
| ------------------ | ---------------------------------- |
| **Constant**       | Use fixed value for all seeds      |
| **Seed Attribute** | Read from seed point attribute     |
| **Vtx Attribute**  | Read from cluster vertex attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Num Iterations Constant</strong> <code>int32</code></summary>

Fixed number of growth iterations.

Default: `3`

📋 _Visible when Num Iterations = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Num Iterations Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute containing iteration count (converted to int32).

📋 _Visible when Num Iterations = Seed/Vtx Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Num Iterations Update Mode</strong> <code>EPCGExGrowthUpdateMode</code></summary>

How iteration count updates during growth.

| Option                 | Description                            |
| ---------------------- | -------------------------------------- |
| **Once**               | Read once at start                     |
| **Set Each Iteration** | Replace remaining iterations each step |
| **Add Each Iteration** | Add to remaining iterations each step  |

Default: `Once`

📋 _Visible when Num Iterations = Vtx Attribute_

⚡ PCG Overridable

</details>

#### Branching

<details>

<summary><strong>Seed Num Branches</strong> <code>EPCGExGrowthValueSource</code></summary>

Source for number of growth branches per seed.

Default: `Constant`

</details>

<details>

<summary><strong>Seed Num Branches Mean</strong> <code>EPCGExMeanMeasure</code></summary>

How branch count relates to available neighbors.

| Option       | Description                               |
| ------------ | ----------------------------------------- |
| **Discrete** | Use exact branch count                    |
| **Relative** | Interpret as ratio of available neighbors |

Default: `Discrete`

</details>

<details>

<summary><strong>Num Branches Constant</strong> <code>int32</code></summary>

Fixed number of branches per seed.

Default: `1`

📋 _Visible when Seed Num Branches = Constant_

⚡ PCG Overridable

</details>

#### Growth Direction

<details>

<summary><strong>Growth Direction</strong> <code>EPCGExGrowthValueSource</code></summary>

Source for preferred growth direction.

Default: `Constant`

</details>

<details>

<summary><strong>Growth Direction Constant</strong> <code>FVector</code></summary>

Fixed growth direction vector.

Default: `Up (0, 0, 1)`

📋 _Visible when Growth Direction = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Growth Direction Update Mode</strong> <code>EPCGExGrowthUpdateMode</code></summary>

How direction updates during growth.

Default: `Once`

</details>

#### Growth Distance

<details>

<summary><strong>Growth Max Distance</strong> <code>EPCGExGrowthValueSource</code></summary>

Source for maximum growth distance.

Default: `Constant`

</details>

<details>

<summary><strong>Growth Max Distance Constant</strong> <code>double</code></summary>

Maximum distance a growth can travel.

Default: `500`

📋 _Visible when Growth Max Distance = Constant_

⚡ PCG Overridable

</details>

#### Limits

<details>

<summary><strong>Use Growth Stop</strong> <code>bool</code></summary>

When enabled, uses an attribute to mark vertices where growth terminates.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Growth Stop Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Boolean attribute marking stop points. Growth ends when reaching these vertices.

📋 _Visible when Use Growth Stop is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use No Growth</strong> <code>bool</code></summary>

When enabled, uses an attribute to mark vertices that growth cannot traverse.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>No Growth Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Boolean attribute marking no-growth points. Paths cannot pass through these vertices.

📋 _Visible when Use No Growth is enabled_

⚡ PCG Overridable

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

#### Advanced

<details>

<summary><strong>Statistics</strong> <code>FPCGExPathStatistics</code></summary>

Outputs usage statistics to cluster attributes.

</details>

<details>

<summary><strong>Use Octree Search</strong> <code>bool</code></summary>

Uses octree spatial indexing for nearest node queries.

Default: `false`

</details>

#### Inherited Settings

→ See Clusters Processor Settings for common cluster processing settings.

### Outputs

| Pin       | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| **Paths** | Points | Generated growth paths                          |
| **Vtx**   | Points | Modified vertices (with usage stats if enabled) |
| **Edges** | Points | Modified edges (with usage stats if enabled)    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Elements/PCGExPathfindingGrowPaths.h)
