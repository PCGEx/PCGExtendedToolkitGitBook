---
description: Extract paths from navmesh.
icon: circle
---

# Pathfinding : Navmesh

### Overview

This node generates paths between seed and goal points using Unreal Engine's built-in navigation mesh system. Unlike cluster-based pathfinding, this uses the level's pre-computed navmesh for pathfinding, making it suitable for game-world navigation that respects level geometry and navigation settings.

### How It Works

1. **Match Seeds to Goals**: Uses the goal picker to pair seed points with goal points.
2. **Query Navmesh**: Finds paths through the navigation mesh for each pair.
3. **Process Points**: Applies fusing and blending to path points.
4. **Output Paths**: Creates path data with forwarded attributes and tags.

**Usage Notes**

* **Requires Navmesh**: The level must have a valid navigation mesh built.
* **Nav Agent**: Respects navigation agent properties (size, capabilities).
* **Navigable Endpoints**: Can require endpoints to be on navigable surfaces.
* **Attribute Forwarding**: Seed and goal attributes can be transferred to paths.

### Behavior

**Navmesh Path Generation:**

```
Level with navmesh (navigable areas shown):
    ┌─────────────────────────────┐
    │  S1      ████████      G1   │
    │          ████████           │
    │  ▓▓▓▓▓▓▓▓████████▓▓▓▓▓▓▓▓  │
    │          ████████           │
    │  S2      ████████      G2   │
    └─────────────────────────────┘

    ████ = Non-navigable obstacle
    ▓▓▓▓ = Navmesh path around obstacle

Output: Paths following navmesh around obstacles
   S1 → ... → G1 (navigates around obstacle)
   S2 → ... → G2 (navigates around obstacle)
```

### Inputs

| Pin       | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| **Seeds** | Points | Starting points for paths    |
| **Goals** | Points | Destination points for paths |

### Settings

#### Goal Picker

<details>

<summary><strong>Goal Picker</strong> <code>UPCGExGoalPicker</code></summary>

Controls how seed points are matched with goal points. Uses the same goal picker system as cluster pathfinding.

Available: Default (by index), All, Random, Index Attribute

⚡ PCG Overridable

</details>

#### Path Composition

<details>

<summary><strong>Add Seed to Path</strong> <code>bool</code></summary>

When enabled, includes the original seed point at the beginning of each path.

Default: `true`

</details>

<details>

<summary><strong>Add Goal to Path</strong> <code>bool</code></summary>

When enabled, includes the original goal point at the end of each path.

Default: `true`

</details>

<details>

<summary><strong>Require Navigable End Location</strong> <code>bool</code></summary>

When enabled, pathfinding fails if the goal location is not on a navigable surface.

Default: `true`

</details>

<details>

<summary><strong>Fuse Distance</strong> <code>double</code></summary>

Minimum distance between consecutive path points. Points closer than this are merged together.

Default: `10`

</details>

#### Blending

<details>

<summary><strong>Blending</strong> <code>UPCGExSubPointsBlendInstancedFactory</code></summary>

Controls how attributes are interpolated along the path from seed to goal.

Available: Inherit Start, Inherit End, None

⚡ PCG Overridable

</details>

#### Tagging & Forwarding

<details>

<summary><strong>Seed Attributes to Path Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copies specified attributes from seed points as tags on output paths.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Seed Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Controls which seed point attributes are forwarded to path points.

//→ See TODO FPCGExForwardDetails

</details>

<details>

<summary><strong>Goal Attributes to Path Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copies specified attributes from goal points as tags on output paths.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Goal Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Controls which goal point attributes are forwarded to path points.

//→ See TODO FPCGExForwardDetails

</details>

#### Advanced

<details>

<summary><strong>Pathfinding Mode</strong> <code>EPCGExPathfindingNavmeshMode</code></summary>

The navmesh query mode to use.

| Option           | Description                                       |
| ---------------- | ------------------------------------------------- |
| **Regular**      | Standard navmesh pathfinding                      |
| **Hierarchical** | Uses hierarchical pathfinding for large distances |

Default: `Regular`

</details>

<details>

<summary><strong>Nav Agent Properties</strong> <code>FNavAgentProperties</code></summary>

Properties of the navigation agent used for pathfinding. Affects which areas of the navmesh are traversable based on agent size and capabilities.

</details>

### Outputs

| Pin       | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| **Paths** | Points | Generated paths following the navmesh |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfindingNavmesh-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfindingNavmesh/Public/Elements/PCGExPathfindingNavmesh.h)
