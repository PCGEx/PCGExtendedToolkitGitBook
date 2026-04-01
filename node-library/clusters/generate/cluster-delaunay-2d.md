---
description: Create a 2D delaunay triangulation for each input dataset.
icon: share-nodes
---

# Cluster : Delaunay 2D

### Overview

This node computes a 2D Delaunay triangulation of each input point set by projecting points onto a plane. The Delaunay triangulation maximizes the minimum angle of all triangles, avoiding thin triangles and producing a well-structured mesh. No point lies inside the circumcircle of any triangle.

### How It Works

1. **Projection**: Projects input points onto a 2D plane using the specified projection settings
2. **Triangulation**: Computes the 2D Delaunay triangulation of the projected points
3. **Edge Extraction**: Creates cluster edges from the triangle connectivity
4. **Urquhart Pruning** (optional): Removes the longest edge from each triangle to create a sparser graph
5. **Hull Marking** (optional): Identifies vertices and edges on the convex hull boundary

**Usage Notes**

* **Minimum Points**: Requires at least 3 non-collinear points (after projection) to form valid triangles.
* **Urquhart Graph**: Removing the longest edge from each triangle produces a graph that tends to connect nearby points while avoiding long spanning edges.
* **Site Merging**: When using Urquhart with sites output, adjacent sites can be merged using either the site centroids or the removed edge midpoints.

### Behavior

```
Input Points              Output Cluster
(3D scattered)            (2D triangulated)

    •     •                   •───•
   •    •        →           /│\ /│\
  •  •    •                 •─┼─•─┼─•
    •                        \│/ \│/
                              •───•

Projected and triangulated
with Delaunay algorithm
```

***

#### Urquhart Mode

> Enable **Urquhart** to transform the dense Delaunay triangulation into a cleaner, more organic-looking graph with minimal effort.

The Urquhart graph removes the longest edge from each triangle, which eliminates the "crossing" edges that make standard Delaunay look overly connected. The result is a graph that:

* Preserves local connectivity while removing long-distance edges
* Creates more interesting, irregular cell shapes when used with face enumeration
* Produces natural-looking networks without additional filtering

```
Delaunay                  Urquhart
(all edges)               (longest removed)

    •───•                     •───•
   /│\ /│\                   / \ / \
  •─┼─•─┼─•      →          •───•───•
   \│/ \│/                   \ / \ /
    •───•                     •───•

Dense triangles           Cleaner polygons
```

When combined with **Output Sites**, the Urquhart mode also allows merging adjacent triangle circumcenters, producing simplified site positions that better represent the resulting cell structure.

***

### Inputs

| Pin    | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| **In** | Points | Input point sets to triangulate |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Urquhart</strong> <code>bool</code></summary>

Output the Urquhart graph of the Delaunay triangulation. This removes the longest edge of each Delaunay triangle, creating a sparser connectivity graph.

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

#### Output Settings

<details>

<summary><strong>Output Sites</strong> <code>bool</code></summary>

Output the Delaunay sites (circumcenters of triangles) as a separate point collection.

Default: `false`

</details>

<details>

<summary><strong>Mark Site Hull</strong> <code>bool</code></summary>

Mark site points that correspond to hull triangles with a boolean attribute.

Default: `false`

</details>

<details>

<summary><strong>Site Hull Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to output the site hull boolean to.

Default: `bIsOnHull`

_Visible when Mark Site Hull = true_

</details>

<details>

<summary><strong>Urquhart Sites Merge</strong> <code>EPCGExUrquhartSiteMergeMode</code></summary>

How to merge adjacent sites when using Urquhart mode with site output.

| Option          | Description                                              |
| --------------- | -------------------------------------------------------- |
| **None**        | Do not merge sites                                       |
| **Merge Sites** | Merge site is the average of the adjacent site positions |
| **Merge Edges** | Merge site is the average of the removed edge midpoints  |

Default: `None`

_Visible when Urquhart = true and Output Sites = true_

</details>

#### Projection Details

<details>

<summary><strong>Method</strong> <code>EPCGExProjectionMethod</code></summary>

How to project 3D points for 2D triangulation.

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

| Pin        | Type   | Description                               |
| ---------- | ------ | ----------------------------------------- |
| **Vtx**    | Points | Cluster vertices (all input points)       |
| **Edges**  | Points | Edge data from the Delaunay triangulation |
| **Labels** | Points | Triangle label points                     |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildDelaunayGraph2D.h)
