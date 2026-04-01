---
description: Create a 3D Convex Hull triangulation for each input dataset.
icon: share-nodes
---

# Cluster : Convex Hull 3D

### Overview

This node computes the 3D convex hull of each input point set, producing a cluster where vertices are the hull surface points and edges form the triangulated hull faces. The convex hull represents the smallest convex shape that encloses all input points, like shrink-wrapping a set of points in 3D space.

### How It Works

1. **Delaunay Computation**: Computes a 3D Delaunay tetrahedralization of the input points
2. **Hull Extraction**: Extracts the outer boundary faces that form the convex hull surface
3. **Graph Construction**: Creates a cluster where hull vertices are connected by edges along the triangulated faces

**Usage Notes**

* **Minimum Points**: Requires at least 4 non-coplanar points to form a valid 3D convex hull.
* **Surface Only**: Only points that lie on the convex hull boundary become vertices in the output; interior points are excluded.
* **Triangulated Output**: The hull surface is represented as triangular faces, with edges connecting adjacent vertices.

### Behavior

```
Input Points              Output Cluster
(scattered 3D)            (hull surface)

    •                         ▲
   •  •                      /|\
  • •   •        →          / | \
   •  •                    /__|__\
    •                     Hull edges form
                          triangulated surface
```

### Inputs

| Pin    | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| **In** | Points | Input point sets to compute convex hulls from |

### Settings

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Points Processor Settings for inherited options.

### Outputs

| Pin        | Type   | Description                              |
| ---------- | ------ | ---------------------------------------- |
| **Vtx**    | Points | Convex hull vertices                     |
| **Edges**  | Points | Edge data connecting hull vertices       |
| **Labels** | Points | Face label points for the hull triangles |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildConvexHull.h)
