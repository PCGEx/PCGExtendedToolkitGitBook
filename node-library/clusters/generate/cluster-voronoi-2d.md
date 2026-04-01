---
description: Create a 2D Voronoi graph for each input dataset.
icon: share-nodes
---

# Cluster : Voronoi 2D

### Overview

This node is effectively **three distinct algorithms in one**, controlled by the Metric setting. Each distance metric produces fundamentally different Voronoi structures:

* **Euclidean** - The classic Voronoi with organic, curved cell boundaries
* **Manhattan** (L1) - Produces 45° diagonal edges and diamond-shaped cells
* **Chebyshev** (L∞) - Produces axis-aligned edges and square-shaped cells

The non-Euclidean metrics (Manhattan and Chebyshev) create visually striking grid-like patterns, but their axis-aligned nature means edges can become perfectly collinear or overlapping in certain point configurations.

### How It Works

1. **Projection**: Projects input points onto a 2D plane using the specified projection settings
2. **Voronoi Construction**: Computes the Voronoi diagram using the selected distance metric
3. **Cell Positioning**: Places vertices at cell centers using the specified method (centroid, circumcenter, or balanced)
4. **Graph Construction**: Creates edges between vertices whose Voronoi cells share an edge
5. **Site Output** (optional): Outputs the original seed points with additional cell metrics

{% hint style="warning" %}
## Non-Euclidean Metrics Require Edge Fusing

Manhattan and Chebyshev metrics produce axis-aligned edges that can result in **perfectly overlapping edges** when points align on grid axes. This creates degenerate topology that breaks DCEL-based operations like cell enumeration.

**Before using the output with face enumeration or other DCEL operations**, run the cluster through a **Fuse Clusters** node to merge overlapping edges into clean topology.
{% endhint %}

**Usage Notes**

* **Minimum Points**: Requires at least 3 non-collinear points (after projection) to form valid Voronoi cells.
* **Open Cells**: Cells on the boundary extend to infinity; use Prune Out Of Bounds to remove vertices from these unbounded cells.
* **Metric Choice**: Euclidean is safe for all downstream operations; Manhattan/Chebyshev may need topology cleanup.

### Behavior

```
Euclidean                 Manhattan                 Chebyshev
(organic curves)          (45° diagonals)           (axis-aligned)

    •───•                     •                         •───•
   /     \                   /|\                        |   |
  •       •                 • • •                       •   •
   \     /                   \|/                        |   |
    •───•                     •                         •───•
```

***

#### Distance Metrics

The **Metric** setting fundamentally changes the Voronoi algorithm:

| Metric        | Distance Formula  | Visual Character                  | Topology                   |
| ------------- | ----------------- | --------------------------------- | -------------------------- |
| **Euclidean** | √(x² + y²)        | Organic, curved boundaries        | Clean, always valid        |
| **Manhattan** | \|x\| + \|y\|     | 45° diagonal edges, diamond cells | May have overlapping edges |
| **Chebyshev** | max(\|x\|, \|y\|) | Axis-aligned edges, square cells  | May have overlapping edges |

***

### Inputs

| Pin    | Type   | Description                      |
| ------ | ------ | -------------------------------- |
| **In** | Points | Input point sets (Voronoi seeds) |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Metric</strong> <code>EPCGExVoronoiMetric</code></summary>

Distance metric used for the Voronoi diagram calculation.

| Option        | Description                                                     |
| ------------- | --------------------------------------------------------------- |
| **Euclidean** | Standard distance - classic Voronoi with curved cell boundaries |
| **Manhattan** | L1 distance - produces 45° diagonal edges                       |
| **Chebyshev** | L∞ distance - produces axis-aligned edges                       |

Default: `Euclidean`

</details>

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

Amount to expand the input bounds for point pruning and balanced centroid calculation.

Default: `100`

</details>

<details>

<summary><strong>Prune Out Of Bounds</strong> <code>bool</code></summary>

Remove cell vertices that fall outside the expanded bounds.

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

Name of the attribute to output the hull boolean to.

Default: `bIsOnHull`

_Visible when Mark Hull = true_

</details>

<details>

<summary><strong>Mark Edge On Touch</strong> <code>bool</code></summary>

When enabled, edges that have at least one endpoint on the hull are also marked as being on the hull.

Default: `false`

</details>

#### Projection Details

<details>

<summary><strong>Method</strong> <code>EPCGExProjectionMethod</code></summary>

How to project 3D points for 2D Voronoi calculation.

Default: `Normal`

</details>

<details>

<summary><strong>Projection Vector</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

The direction vector used for projection.

Default: `Up Vector (0, 0, 1)`

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Additional Outputs

<details>

<summary><strong>Output Sites</strong> <code>bool</code></summary>

Output the original seed points with additional Voronoi cell metrics.

Default: `true`

</details>

<details>

<summary><strong>Prune Open Sites</strong> <code>bool</code></summary>

Remove sites that belong to cells that were pruned (out-of-bounds).

Default: `true`

_Visible when Output Sites = true and Prune Out Of Bounds = true_

</details>

<details>

<summary><strong>Open Site Flag</strong> <code>FName</code></summary>

Attribute name to flag sites belonging to open (unbounded) cells.

Default: `OpenSite`

_Visible when Output Sites = true, Prune Out Of Bounds = true, and Prune Open Sites = false_

</details>

**Sites Output Details**

<details>

<summary><strong>Write Influences Count</strong> <code>bool</code></summary>

Write the number of Voronoi cell vertices influenced by each site.

Default: `false`

</details>

<details>

<summary><strong>Write Min Radius</strong> <code>bool</code></summary>

Write the minimum distance from each site to its cell vertices.

Default: `false`

</details>

<details>

<summary><strong>Write Max Radius</strong> <code>bool</code></summary>

Write the maximum distance from each site to its cell vertices.

Default: `false`

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildVoronoiGraph2D.h)
