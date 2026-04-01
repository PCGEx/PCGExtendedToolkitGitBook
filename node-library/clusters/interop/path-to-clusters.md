---
description: Merge paths to edge clusters for glorious pathfinding inception.
icon: share-nodes
---

# Path : To Clusters

### Overview

This node converts path point collections into cluster graphs. Each path becomes a chain of connected edges, and when fusing is enabled, overlapping paths are merged into a unified graph with proper intersection handling.

This is a **key node for the Cluster ↔ Path workflow cycle**: pathfinding operations output paths, but many PCGEx operations require clusters. This node bridges that gap, allowing pathfinding results to be re-integrated into the cluster ecosystem for further operations.

### How It Works

1. **Path Processing**: Converts each path into a sequence of vertices and edges
2. **Fusing** (optional): Merges overlapping points based on proximity
3. **Point-Edge Intersections** (optional): Detects where points lie on edges and splits accordingly
4. **Edge-Edge Intersections** (optional): Detects edge crossings and creates intersection vertices
5. **Attribute Blending**: Merges attributes when points or edges are fused together
6. **Graph Output**: Produces a unified cluster graph

**Usage Notes**

* **Fusing Enabled**: Paths that share points (within fuse distance) become connected, creating a unified navigable network.
* **Fusing Disabled**: Each path becomes an independent cluster with no connections to other paths.
* **Intersection Detection**: Enable Point-Edge and Edge-Edge intersections for complete topology but at increased cost.

### Behavior

```
WITHOUT FUSING:                 WITH FUSING:
Path A: ●───●───●               ●───●───●
Path B: ●───●───●                   │
        (overlapping)           ●───●───●
                                (shared vertex)

WITH EDGE-EDGE INTERSECTIONS:
    ●                               ●
    │                               │
●───┼───●           ══►         ●───●───●
    │                               │
    ●                               ●
(paths cross)                  (intersection vertex)
```

### Inputs

| Pin    | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| **In** | Points | Path point collections to convert |

### Settings

#### Fusing

<details>

<summary><strong>Fuse Paths</strong> <code>bool</code></summary>

Whether to fuse overlapping paths into a single graph or keep them separate.

Default: `true`

</details>

<details>

<summary><strong>Point/Point Settings</strong> <code>FPCGExPointPointIntersectionDetails</code></summary>

Settings for fusing overlapping points from different paths.

* **Fuse Details**: Distance threshold and component tolerances for point matching
* **Point Union Data**: Metadata to write about fused points
* **Edge Union Data**: Metadata to write about merged edges

📋 _Visible when Fuse Paths = true_

</details>

#### Intersection Detection

<details>

<summary><strong>Find Point-Edge Intersections</strong> <code>bool</code></summary>

Detect where points lie on edges from other paths and create proper T-junction connections.

Default: `false`

📋 _Visible when Fuse Paths = true_

</details>

<details>

<summary><strong>Point/Edge Settings</strong> <code>FPCGExPointEdgeIntersectionDetails</code></summary>

Settings for point-on-edge detection.

* **Enable Self Intersection**: Check within the same path
* **Snap On Edge**: Snap the point to the edge it intersects
* **Write Is Intersector**: Mark intersecting points with an attribute

📋 _Visible when Find Point-Edge Intersections = true_

</details>

<details>

<summary><strong>Find Edge-Edge Intersections</strong> <code>bool</code></summary>

Detect where edges from different paths cross and create intersection vertices.

Default: `false`

📋 _Visible when Fuse Paths = true_

</details>

<details>

<summary><strong>Edge/Edge Settings</strong> <code>FPCGExEdgeEdgeIntersectionDetails</code></summary>

Settings for edge crossing detection.

* **Enable Self Intersection**: Check within the same path
* **Tolerance**: Distance threshold for intersection detection
* **Min/Max Angle**: Filter crossings by angle
* **Write Crossing**: Mark crossing points with an attribute

📋 _Visible when Find Edge-Edge Intersections = true_

</details>

#### Data Blending

<details>

<summary><strong>Default Points Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

Defines how point properties and attributes are merged when points are fused.

📋 _Visible when Fuse Paths = true_

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Default Edges Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

Defines how edge properties and attributes are merged when edges are fused.

📋 _Visible when Fuse Paths = true_

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Use Custom Point-Edge Blending</strong> <code>bool</code></summary>

Use separate blending settings for Point-Edge intersections.

Default: `false`

</details>

<details>

<summary><strong>Use Custom Edge-Edge Blending</strong> <code>bool</code></summary>

Use separate blending settings for Edge-Edge intersections (crossings).

Default: `false`

</details>

#### Output Settings

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are preserved from input paths to output clusters.

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Graph and edge output properties for the generated cluster.

</details>

### Outputs

| Pin       | Type   | Description           |
| --------- | ------ | --------------------- |
| **Vtx**   | Points | Cluster vertex points |
| **Edges** | Points | Cluster edge data     |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Paths/PCGExPathToClusters.h)
