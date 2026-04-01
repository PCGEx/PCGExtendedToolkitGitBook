---
description: >-
  Configures how external points (seeds, goals) are matched to the nearest node
  or edge within a cluster.
icon: sliders-simple
---

# Node Selection Details

### Overview

This settings block controls how pathfinding and cell-finding nodes locate their starting or target positions within a cluster. When you provide seed or goal points that don't exactly match cluster vertices, these settings determine whether matching occurs by finding the closest vertex or the closest point along an edge, and how far away a match is allowed to be.

### How It Works

1. **Receive Query Point**: Takes an external point position (seed, goal, etc.)
2. **Search Cluster**: Finds the nearest vertex or edge based on the picking method
3. **Distance Check**: If Max Distance is set, rejects matches beyond that threshold
4. **Return Node**: Returns the matched cluster node index for further processing

### Behavior

```
Query Point (●) → Search Cluster → Matched Node

Picking Method = Vtx:
    ●───────────○ Vtx A
         └──────○ Vtx B  ← Closest vertex selected

Picking Method = Edge:
    ●─────×─────○───────○
          ↑
    Closest point on edge selected
    (returns nearest endpoint of that edge)
```

### Settings

<details>

<summary><strong>Picking Method</strong> <code>EPCGExClusterClosestSearchMode</code></summary>

Determines how the nearest cluster position is found.

| Option   | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| **Vtx**  | Finds the closest vertex directly                                           |
| **Edge** | Finds the closest point on any edge, then uses that edge's nearest endpoint |

Default: `Edge`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Distance</strong> <code>double</code></summary>

Maximum allowed distance between the query point and matched cluster position. Points farther than this threshold will fail to match.

Set to `-1` to disable the distance limit (any distance is accepted).

Default: `-1`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Clusters/PCGExClusterCommon.h)
