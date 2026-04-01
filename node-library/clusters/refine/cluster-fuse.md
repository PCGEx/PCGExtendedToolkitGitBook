---
description: Finds Point/Edge and Edge/Edge intersections between all input clusters.
icon: share-nodes
---

# Cluster : Fuse

### Overview

This node merges multiple cluster graphs by detecting and resolving intersections between them. It fuses overlapping vertices, finds where points lie on edges, and detects edge crossings — creating new vertices at intersection points and properly connecting the merged topology.

This is a **key node for combining cluster data**: whether merging modular graph segments, integrating pathfinding results (via Path to Clusters) back into a master graph, or building composite networks from multiple sources, Fuse Clusters handles the complex intersection detection automatically.

### How It Works

1. **Point-Point Fusing**: Merges vertices within the fuse distance into shared junctions
2. **Point-Edge Detection** (optional): Finds where vertices lie on edges from other clusters, splits those edges, and creates T-junction connections
3. **Edge-Edge Detection** (optional): Finds where edges cross, creates intersection vertices, and splits both edges appropriately
4. **Attribute Blending**: Merges attributes from fused elements using configurable blending
5. **Graph Rebuild**: Outputs a unified cluster with all intersections resolved

**Usage Notes**

* **Fuse Distance**: Configure the tolerance for considering points as overlapping.
* **Self-Intersection**: Each intersection type can optionally check within the same cluster.
* **Attribute Blending**: Different blending settings can be used for different intersection types.

### Behavior

```
POINT-POINT FUSING:             POINT-EDGE INTERSECTIONS:
Cluster A: ●───●───●            Cluster A: ●───────────●
               ↑
Cluster B: ●───●───●            Cluster B:     ●
           (overlapping)                       │
                                               ●
Unified:   ●───●───●            Unified:   ●─────●─────●
               │                                 │
           ●───●───●                             ●
           (shared vertex)                  (T-junction)

EDGE-EDGE INTERSECTIONS:
Cluster A: ●───────●            Unified:       ●
                                               │
Cluster B:     ●                           ●───●───●
               │                               │
               ●                               ●
           (edges cross)                (crossroad vertex)
```

### Inputs

| Pin       | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| **Vtx**   | Points | Cluster vertex points from multiple clusters |
| **Edges** | Points | Cluster edge data from multiple clusters     |

### Settings

#### Point-Point Intersection

<details>

<summary><strong>Point/Point Settings</strong> <code>FPCGExPointPointIntersectionDetails</code></summary>

Settings for fusing overlapping vertices.

* **Fuse Details**: Distance tolerance and per-component settings
* **Point Union Data**: Metadata to write about fused points
* **Edge Union Data**: Metadata to write about merged edges

</details>

#### Point-Edge Intersection

<details>

<summary><strong>Find Point-Edge Intersections</strong> <code>bool</code></summary>

Detect where vertices lie on edges from other clusters, creating T-junction connections.

Default: `false`

</details>

<details>

<summary><strong>Point/Edge Settings</strong> <code>FPCGExPointEdgeIntersectionDetails</code></summary>

Settings for point-on-edge detection.

* **Enable Self Intersection**: Check within the same cluster
* **Snap On Edge**: Move the point to lie exactly on the edge
* **Write Is Intersector**: Mark points that caused edge splits

📋 _Visible when Find Point-Edge Intersections = true_

</details>

#### Edge-Edge Intersection

<details>

<summary><strong>Find Edge-Edge Intersections</strong> <code>bool</code></summary>

Detect where edges cross each other and create intersection vertices.

Default: `false`

</details>

<details>

<summary><strong>Edge/Edge Settings</strong> <code>FPCGExEdgeEdgeIntersectionDetails</code></summary>

Settings for edge crossing detection.

* **Enable Self Intersection**: Check within the same cluster
* **Tolerance**: Distance threshold for intersection detection
* **Min/Max Angle**: Filter crossings by angle
* **Write Crossing**: Mark crossing points with an attribute
* **Flag Crossing**: Mark edges involved in crossings

📋 _Visible when Find Edge-Edge Intersections = true_

</details>

#### Data Blending

<details>

<summary><strong>Default Points Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

How to merge attributes when vertices are fused together.

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Default Edges Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

How to merge attributes when edges are fused together.

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Use Custom Point-Edge Blending</strong> <code>bool</code></summary>

Use separate blending settings for point-edge intersections.

Default: `false`

</details>

<details>

<summary><strong>Use Custom Edge-Edge Blending</strong> <code>bool</code></summary>

Use separate blending settings for edge-edge intersections (crossings).

Default: `false`

</details>

#### Meta Filters

<details>

<summary><strong>Carry Over Settings - Vtx</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which vertex attributes and tags are preserved during fusion.

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Carry Over Settings - Edges</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which edge attributes and tags are preserved during fusion.

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Graph and edge output properties for the unified cluster.

</details>

### Outputs

| Pin       | Type   | Description              |
| --------- | ------ | ------------------------ |
| **Vtx**   | Points | Unified cluster vertices |
| **Edges** | Points | Unified cluster edges    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExFuseClusters.h)
