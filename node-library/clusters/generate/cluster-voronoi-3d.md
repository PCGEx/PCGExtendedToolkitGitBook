---
description: Create a 3D Voronoi graph for each input dataset.
icon: share-nodes
---

# Cluster : Voronoi 3D

### Overview

This node computes a 3D Voronoi diagram from each input point set, producing a cluster where vertices represent Voronoi cell locations and edges connect adjacent cells. Each Voronoi cell contains all points in space closer to its seed point than to any other seed, and the graph captures the adjacency relationships between these cells.

### How It Works

1. **Delaunay Construction**: Computes a 3D Delaunay tetrahedralization of the input points
2. **Voronoi Extraction**: Derives the Voronoi diagram as the dual of the Delaunay structure
3. **Cell Positioning**: Places vertices at cell centers using the specified method (centroid, circumcenter, or balanced)
4. **Graph Construction**: Creates edges between vertices whose Voronoi cells share a face

**Usage Notes**

* **Minimum Points**: Requires at least 4 non-coplanar points to form valid 3D Voronoi cells.
* **Unbounded Cells**: Cells on the convex hull boundary extend to infinity; the Method and ExpandBounds settings control how these are handled.
* **Circumcenter vs Centroid**: Circumcenters are mathematically precise but can lie far outside the cell; centroids are always inside but may not represent the true Voronoi vertex.

### Behavior

```
Input Points              Output Cluster
(seed positions)          (cell adjacency)

    •                         ┌─•─┐
   • •                        │ │ │
  •   •          →            •─┼─•
   • •                        │ │ │
    •                         └─•─┘

Points become cell seeds.
Adjacent cells are connected.
```

### Inputs

| Pin    | Type   | Description                      |
| ------ | ------ | -------------------------------- |
| **In** | Points | Input point sets (Voronoi seeds) |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Method</strong> <code>EPCGExCellCenter</code></summary>

Method used to determine where each Voronoi cell vertex is placed.

| Option           | Description                                                                        |
| ---------------- | ---------------------------------------------------------------------------------- |
| **Centroid**     | Use the geometric center of the cell vertices - always inside the cell             |
| **Circumcenter** | Use the Delaunay circumcenter - mathematically precise but may be outside the cell |
| **Balanced**     | Blend between centroid and circumcenter, clamped to bounds                         |

Default: `Centroid`

</details>

<details>

<summary><strong>Expand Bounds</strong> <code>double</code></summary>

Amount to expand the input bounds for point pruning and balanced centroid calculation. Larger values allow more distant cell centers.

Default: `100`

</details>

<details>

<summary><strong>Prune Out Of Bounds</strong> <code>bool</code></summary>

Remove cell vertices that fall outside the expanded bounds. Useful when using Circumcenter method to eliminate distant outliers.

Default: `false`

_Visible when Method = Circumcenter_

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

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Points Processor Settings for inherited options.

### Outputs

| Pin        | Type   | Description                            |
| ---------- | ------ | -------------------------------------- |
| **Vtx**    | Points | Voronoi cell center vertices           |
| **Edges**  | Points | Edges connecting adjacent cell centers |
| **Labels** | Points | Cell label points                      |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildVoronoiGraph.h)
