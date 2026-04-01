---
description: Create a 3D delaunay tetrahedralization for each input dataset.
icon: share-nodes
---

# Cluster : Delaunay 3D

### Overview

This node computes a 3D Delaunay tetrahedralization of each input point set, creating a cluster where all points are connected in a way that maximizes the minimum angle of all tetrahedra. The Delaunay triangulation produces well-shaped tetrahedra and has the property that no point lies inside the circumsphere of any tetrahedron.

### How It Works

1. **Tetrahedralization**: Computes the 3D Delaunay tetrahedralization connecting all input points
2. **Edge Extraction**: Extracts unique edges from all tetrahedra to form the cluster connectivity
3. **Urquhart Pruning** (optional): Removes the longest edge from each tetrahedron to create a sparser graph
4. **Hull Marking** (optional): Identifies and marks vertices and edges that lie on the convex hull boundary

**Usage Notes**

* **Minimum Points**: Requires at least 4 non-coplanar points to form valid tetrahedra.
* **Urquhart Graph**: The Urquhart graph is a subgraph of Delaunay that removes the longest edge from each tetrahedron, often producing more natural-looking connections.
* **Sites Output**: Sites represent the circumcenters of the Delaunay tetrahedra - useful for generating Voronoi-like structures.

### Behavior

```
Input Points              Output Cluster
(scattered 3D)            (tetrahedralized)

    •                         •
   •  •                      /|\
  • •   •        →          •-•-•
   •  •                      \|/
    •                         •

All points connected via
Delaunay tetrahedralization
```

### Inputs

| Pin    | Type   | Description                        |
| ------ | ------ | ---------------------------------- |
| **In** | Points | Input point sets to tetrahedralize |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Urquhart</strong> <code>bool</code></summary>

Output the Urquhart graph of the Delaunay triangulation. This removes the longest edge of each Delaunay tetrahedron, creating a sparser connectivity graph.

Default: `false`

</details>

<details>

<summary><strong>Mark Hull</strong> <code>bool</code></summary>

Mark vertices and edges that lie on the convex hull boundary with a boolean attribute.

Default: `false`

</details>

<details>

<summary><strong>Hull Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to output the hull boolean to. True if the point is on the hull, otherwise false.

Default: `bIsOnHull`

_Visible when Mark Hull = true_

</details>

<details>

<summary><strong>Mark Edge On Touch</strong> <code>bool</code></summary>

When enabled, edges that have at least one endpoint on the hull are also marked as being on the hull.

Default: `false`

</details>

#### Sites Settings

<details>

<summary><strong>Output Sites</strong> <code>bool</code></summary>

Output the Delaunay sites (circumcenters of tetrahedra) as a separate point collection.

Default: `false`

</details>

<details>

<summary><strong>Mark Site Hull</strong> <code>bool</code></summary>

Mark site points that correspond to hull tetrahedra with a boolean attribute.

Default: `false`

</details>

<details>

<summary><strong>Site Hull Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to output the site hull boolean to.

Default: `bIsOnHull`

_Visible when Mark Site Hull = true_

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

| Pin        | Type   | Description                                    |
| ---------- | ------ | ---------------------------------------------- |
| **Vtx**    | Points | Cluster vertices (all input points)            |
| **Edges**  | Points | Edge data from the Delaunay tetrahedralization |
| **Labels** | Points | Tetrahedra label points                        |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildDelaunayGraph.h)
