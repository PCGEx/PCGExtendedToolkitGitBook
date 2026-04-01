---
description: >-
  Creates a graph from cell adjacency relationships. Points are cell centroids,
  edges connect adjacent cells.
icon: share-nodes
---

# Cluster : Cell Diagram

### Overview

This node transforms a cluster's cells into a connectivity graph where each cell becomes a point at its centroid, and edges connect cells that share boundaries. The resulting graph represents the topological relationships between cells, where connected nodes indicate neighboring cells.

### How It Works

1. **Cell Enumeration**: Identifies valid cells from the input cluster based on constraint filters (area, point count, compactness, etc.)
2. **Centroid Computation**: Calculates the centroid position for each valid cell by blending the positions of its boundary vertices
3. **Adjacency Detection**: Determines which cells share edges by analyzing face connectivity in the original cluster
4. **Graph Construction**: Creates output points at cell centroids and edges between adjacent cells

**Usage Notes**

* **Cell-to-Graph Transformation**: This node converts spatial cells into an abstract adjacency graph - use it when you need to reason about cell relationships rather than their geometry.
* **Attribute Blending**: Cell vertex attributes are combined into the centroid using the specified blending mode, allowing cell properties to transfer to the output graph.
* **Compactness Metric**: Compactness measures how close a cell's shape is to a circle (1.0 = perfect circle), calculated as &#x34;_&#x50;&#x49;_&#x41;rea/Perimeter^2.

### Behavior

```
Input Cluster              Output Graph
with Cells                 (Centroids + Edges)

┌─────┬─────┐
│  A  │  B  │                  A ─── B
├─────┼─────┤        →         │     │
│  C  │  D  │                  C ─── D
└─────┴─────┘

Each cell becomes a point at its center.
Adjacent cells are connected by edges.
```

### Inputs

| Pin        | Type   | Description                                   |
| ---------- | ------ | --------------------------------------------- |
| **Vtx**    | Points | Cluster vertex points                         |
| **Edges**  | Points | Cluster edge data                             |
| **Labels** | Points | Optional label points for cell identification |

### Settings

#### Cell Constraints

<details>

<summary><strong>Rotation Method</strong> <code>EPCGExCellRotationMethod</code></summary>

Method used to determine cell orientation.

| Option           | Description                                     |
| ---------------- | ----------------------------------------------- |
| **Projection2D** | Use 2D projection plane for winding calculation |
| **Projection3D** | Use 3D normal-based orientation                 |

Default: `Projection2D`

</details>

<details>

<summary><strong>Output Winding</strong> <code>EPCGExWinding</code></summary>

Target winding order for output cell vertices.

| Option               | Description                        |
| -------------------- | ---------------------------------- |
| **CounterClockwise** | Vertices ordered counter-clockwise |
| **Clockwise**        | Vertices ordered clockwise         |

Default: `CounterClockwise`

</details>

<details>

<summary><strong>Aspect Filter</strong> <code>EPCGExCellShapeTypeOutput</code></summary>

Filter cells by their shape classification (convex, concave, or both).

Default: `Both`

</details>

<details>

<summary><strong>Keep Cells With Leaves</strong> <code>bool</code></summary>

Whether to keep cells that contain leaf vertices (vertices with only one edge connection).

Default: `true`

</details>

<details>

<summary><strong>Duplicate Leaf Points</strong> <code>bool</code></summary>

When keeping cells with leaves, whether to duplicate leaf points to close the cell boundary.

Default: `false`

_Visible when Keep Cells With Leaves = true_

</details>

<details>

<summary><strong>Omit Wrapping Bounds</strong> <code>bool</code></summary>

Exclude cells that form the outer boundary of the cluster (typically large cells wrapping the entire point set).

Default: `true`

</details>

<details>

<summary><strong>Classification Tolerance</strong> <code>double</code></summary>

Tolerance value for classifying cells as wrapping bounds.

Default: `0.1`

_Visible when Omit Wrapping Bounds = true_

</details>

<details>

<summary><strong>Keep if Sole</strong> <code>bool</code></summary>

Keep the wrapping bounds cell if it's the only cell remaining after filtering.

Default: `true`

_Visible when Omit Wrapping Bounds = true_

</details>

<details>

<summary><strong>Omit Below/Above Bounds Size</strong> <code>bool</code></summary>

Filter cells by their bounding box size. Enable to exclude cells smaller or larger than specified thresholds.

Default: `false`

</details>

<details>

<summary><strong>Omit Below/Above Point Count</strong> <code>bool</code></summary>

Filter cells by the number of vertices. Enable to exclude cells with fewer or more vertices than specified.

Default: `false`

</details>

<details>

<summary><strong>Omit Below/Above Area</strong> <code>bool</code></summary>

Filter cells by their computed area. Enable to exclude cells smaller or larger than specified areas.

Default: `false`

</details>

<details>

<summary><strong>Omit Below/Above Perimeter</strong> <code>bool</code></summary>

Filter cells by their perimeter length. Enable to exclude cells with shorter or longer perimeters than specified.

Default: `false`

</details>

<details>

<summary><strong>Omit Below/Above Segment Length</strong> <code>bool</code></summary>

Filter cells by their edge segment lengths. Enable to exclude cells with segments shorter or longer than specified.

Default: `false`

</details>

<details>

<summary><strong>Omit Below/Above Compactness</strong> <code>bool</code></summary>

Filter cells by their compactness ratio (how circular the shape is, from 0 to 1).

Default: `false`

</details>

#### Projection Details

<details>

<summary><strong>Method</strong> <code>EPCGExProjectionMethod</code></summary>

How to project 3D points for 2D cell calculations.

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

#### Attribute Output

<details>

<summary><strong>Write Area</strong> <code>bool</code></summary>

Write the cell's area to the centroid point.

Default: `false`

</details>

<details>

<summary><strong>Area Attribute Name</strong> <code>FName</code></summary>

Attribute name for the area value.

Default: `Area`

_Visible when Write Area = true_

</details>

<details>

<summary><strong>Write Compactness</strong> <code>bool</code></summary>

Write the cell's compactness ratio to the centroid point.

Default: `false`

</details>

<details>

<summary><strong>Compactness Attribute Name</strong> <code>FName</code></summary>

Attribute name for the compactness value.

Default: `Compactness`

_Visible when Write Compactness = true_

</details>

<details>

<summary><strong>Write Num Nodes</strong> <code>bool</code></summary>

Write the number of vertices in the cell to the centroid point.

Default: `false`

</details>

<details>

<summary><strong>Num Nodes Attribute Name</strong> <code>FName</code></summary>

Attribute name for the vertex count.

Default: `NumNodes`

_Visible when Write Num Nodes = true_

</details>

#### Blending Details

<details>

<summary><strong>Blending Filter</strong> <code>EPCGExAttributeFilter</code></summary>

Which attributes to include in blending operations.

| Option      | Description                           |
| ----------- | ------------------------------------- |
| **All**     | Blend all attributes                  |
| **Include** | Only blend specified attributes       |
| **Exclude** | Blend all except specified attributes |

Default: `All`

</details>

<details>

<summary><strong>Default Blending</strong> <code>EPCGExBlendingType</code></summary>

Default blending mode for combining cell vertex values into the centroid.

| Option      | Description           |
| ----------- | --------------------- |
| **Average** | Average of all values |
| **Weight**  | Weighted average      |
| **Min**     | Minimum value         |
| **Max**     | Maximum value         |
| **Sum**     | Sum of all values     |

Default: `Average`

</details>

#### Carry Over Settings

<details>

<summary><strong>Preserve Attributes Default Value</strong> <code>bool</code></summary>

Whether to preserve the default value for attributes when carrying over.

Default: `false`

</details>

<details>

<summary><strong>Attributes</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filter settings for which attributes to carry over from source to output.

//→ See TODO FPCGExNameFiltersDetails

</details>

<details>

<summary><strong>Tags</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filter settings for which tags to carry over from source to output.

//→ See TODO FPCGExNameFiltersDetails

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| **Vtx**   | Points | Centroid points representing each valid cell |
| **Edges** | Points | Edge data connecting adjacent cell centroids |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildCellDiagram.h)
