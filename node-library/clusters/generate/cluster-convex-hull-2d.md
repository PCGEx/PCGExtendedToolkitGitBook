---
description: Create a 2D Convex Hull triangulation for each input dataset.
icon: share-nodes
---

# Cluster : Convex Hull 2D

### Overview

This node computes the 2D convex hull of each input point set by projecting points onto a plane and finding the outer boundary. The result can be output as either a cluster (vertices + edges) or as paths. The convex hull represents the smallest convex polygon that encloses all projected points.

### How It Works

1. **Projection**: Projects input points onto a 2D plane using the specified projection settings
2. **Hull Computation**: Calculates the convex hull boundary of the projected points
3. **Output Generation**: Creates either a cluster with vertices and edges, or a path, with the specified winding order

**Usage Notes**

* **Minimum Points**: Requires at least 3 non-collinear points (after projection) to form a valid hull.
* **Projection Plane**: Points are flattened to 2D using the projection settings; the projection direction determines which axis is ignored.
* **Output Mode**: When outputting as paths instead of clusters, only the boundary vertices are included without edge data.

### Behavior

```
Input Points              Output Cluster
(3D scattered)            (2D hull boundary)

    •     •                   ┌─────┐
   •    •        →            │     │
  •  •    •                   └─────┘
    •                     Projected boundary
                          with specified winding
```

### Inputs

| Pin    | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| **In** | Points | Input point sets to compute convex hulls from |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Output Clusters</strong> <code>bool</code></summary>

Output the hull as a cluster (vertices + edges) instead of paths.

Default: `true`

</details>

<details>

<summary><strong>Winding</strong> <code>EPCGExWinding</code></summary>

Vertex ordering direction for the output hull.

| Option               | Description                        |
| -------------------- | ---------------------------------- |
| **Clockwise**        | Vertices ordered clockwise         |
| **CounterClockwise** | Vertices ordered counter-clockwise |

Default: `CounterClockwise`

</details>

#### Projection Details

<details>

<summary><strong>Method</strong> <code>EPCGExProjectionMethod</code></summary>

How to project 3D points for 2D hull calculation.

| Option     | Description                                |
| ---------- | ------------------------------------------ |
| **Normal** | Project along a specified direction vector |

Default: `Normal`

</details>

<details>

<summary><strong>Projection Vector</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

The direction vector used for projection. Can be a constant or read from an attribute.

Default: `Up Vector (0, 0, 1)`

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Points Processor Settings for inherited options.

### Outputs

| Pin        | Type   | Description                                                      |
| ---------- | ------ | ---------------------------------------------------------------- |
| **Vtx**    | Points | Convex hull vertices                                             |
| **Edges**  | Points | Edge data connecting hull vertices (when Output Clusters = true) |
| **Labels** | Points | Face label points                                                |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildConvexHull2D.h)
