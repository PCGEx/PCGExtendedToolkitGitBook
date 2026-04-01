---
description: Configures statistics tracking for pathfinding operations.
icon: sliders-simple
---

# Path Statistics

### Overview

This settings block enables tracking of how often vertices and edges are used across all discovered paths. By writing use counts to attributes, you can identify high-traffic nodes and edges in the cluster — useful for visualizing path density, finding bottlenecks, or weighting subsequent operations based on path popularity.

### How It Works

1. **Run Pathfinding**: Multiple paths are computed through the cluster
2. **Count Usage**: Track how many paths pass through each vertex and edge
3. **Write Attributes**: Store use counts on the original cluster data
4. **Analyze Results**: Use counts for visualization or downstream processing

### Behavior

```
Path Statistics Example:

Cluster with 3 paths found:
  Path 1: A → B → C → D
  Path 2: A → B → E → D
  Path 3: F → B → C → D

Vertex Use Counts:
  A: 2, B: 3, C: 2, D: 3, E: 1, F: 1

Edge Use Counts:
  A→B: 2, B→C: 2, B→E: 1, C→D: 2, E→D: 1, F→B: 1

High-traffic nodes (B, D) could indicate:
  - Critical junctions
  - Potential bottlenecks
  - Important waypoints
```

### Settings

<details>

<summary><strong>Write Point Use Count</strong> <code>bool</code></summary>

When enabled, writes how many paths pass through each vertex as an attribute on the vertex data.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Point Use Count</strong> <code>FName</code></summary>

Name of the attribute to store vertex use counts.

Default: `PointUseCount`

📋 _Visible when Write Point Use Count = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Edge Use Count</strong> <code>bool</code></summary>

When enabled, writes how many paths traverse each edge as an attribute on the edge data.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Use Count</strong> <code>FName</code></summary>

Name of the attribute to store edge use counts.

Default: `EdgeUseCount`

📋 _Visible when Write Edge Use Count = true_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Core/PCGExPathfinding.h)
