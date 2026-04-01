---
description: >-
  Build the edge dual graph: edges become vertices that connect to sequential
  edges around shared endpoints.
icon: share-nodes
---

# Cluster : Dual Graph

### Overview

This node transforms a cluster by swapping the roles of edges and vertices. Each original edge becomes a vertex in the output, positioned at the edge midpoint. These new vertices connect to other edge-vertices that were sequential around shared endpoints in the original graph. The result is the topological dual of the edge connectivity.

### How It Works

1. **Edge-to-Vertex Conversion**: Each edge in the input cluster becomes a vertex in the output, positioned at the edge midpoint
2. **DCEL Traversal**: Uses half-edge data structure to find edges that share endpoints and are adjacent in rotational order
3. **Dual Edge Creation**: Creates edges between new vertices whose original edges were sequential around a shared vertex
4. **Attribute Blending**: Blends original edge attributes to new vertices, and original vertex attributes to new edges

{% hint style="warning" %}
Because it relies on planar enumeration, it will produce janky results on topologies that can't be projected or unwrapped easily.
{% endhint %}

**Usage Notes**

* **Topology Transformation**: The dual graph inverts the relationship between edges and vertices - useful for analyzing edge connectivity patterns.
* **Rotational Ordering**: Edges connect based on their rotational sequence around shared vertices, determined by the 2D projection.
* **Bidirectional Blending**: Edge attributes can transfer to the new vertices, while vertex attributes (from shared endpoints) can transfer to new edges.

### Behavior

```
Original Cluster          Dual Graph
(vertices + edges)        (edges → vertices)

    A───B                     •
    │\ /│                    /│\
    │ X │        →          • │ •
    │/ \│                    \│/
    C───D                     •

Original edges become vertices.
Sequential edges around shared
points become connected.
```

### Inputs

| Pin       | Type   | Description           |
| --------- | ------ | --------------------- |
| **Vtx**   | Points | Cluster vertex points |
| **Edges** | Points | Cluster edge data     |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Write Edge Length</strong> <code>bool</code></summary>

Write the original edge length to the new vertex points.

Default: `false`

</details>

<details>

<summary><strong>Edge Length Attribute Name</strong> <code>FName</code></summary>

Attribute name for the original edge length value.

Default: `EdgeLength`

_Visible when Write Edge Length = true_

</details>

<details>

<summary><strong>Write Original Edge Index</strong> <code>bool</code></summary>

Write the original edge index to the new vertex points, allowing you to trace back to the source edge.

Default: `false`

</details>

<details>

<summary><strong>Original Edge Index Attribute Name</strong> <code>FName</code></summary>

Attribute name for the original edge index.

Default: `OriginalEdgeIndex`

_Visible when Write Original Edge Index = true_

</details>

#### Projection Details

<details>

<summary><strong>Method</strong> <code>EPCGExProjectionMethod</code></summary>

How to project points for DCEL construction and rotational ordering.

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

<summary><strong>Write Edge Position</strong> <code>bool</code></summary>

Write edge midpoint position as an attribute on edge points.

Default: `true`

</details>

<details>

<summary><strong>Edge Position</strong> <code>double</code></summary>

Normalized position along the edge (0 = start, 0.5 = middle, 1 = end).

Default: `0.5`

_Visible when Write Edge Position = true_

</details>

<details>

<summary><strong>Remove Small Clusters</strong> <code>bool</code></summary>

Remove clusters below a minimum vertex or edge count threshold.

Default: `false`

</details>

<details>

<summary><strong>Remove Big Clusters</strong> <code>bool</code></summary>

Remove clusters above a maximum vertex or edge count threshold.

Default: `false`

</details>

<details>

<summary><strong>Refresh Edge Seed</strong> <code>bool</code></summary>

Regenerate random seeds for edge points.

Default: `false`

</details>

<details>

<summary><strong>Build And Cache Clusters</strong> <code>EPCGExOptionState</code></summary>

Whether to build and cache cluster data for downstream nodes.

| Option       | Description            |
| ------------ | ---------------------- |
| **Default**  | Use default behavior   |
| **Enabled**  | Always build and cache |
| **Disabled** | Never build and cache  |

Default: `Default`

</details>

<details>

<summary><strong>Output Edge Length</strong> <code>bool</code></summary>

Write edge length as an attribute on edge points.

Default: `false`

</details>

#### Blending (Vtx from Edges)

Controls how original edge attributes are blended to the new vertices.

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

Default blending mode for combining edge attribute values.

Default: `None`

</details>

#### Blending (Edges from Vtx)

Controls how original vertex attributes (from shared endpoints) are blended to new edges.

<details>

<summary><strong>Blending Filter</strong> <code>EPCGExAttributeFilter</code></summary>

Which attributes to include in blending operations.

Default: `All`

</details>

<details>

<summary><strong>Default Blending</strong> <code>EPCGExBlendingType</code></summary>

Default blending mode for combining vertex attribute values to edges.

Default: `None`

</details>

#### Carry Over Settings

Both blending categories have their own carry-over settings for controlling attribute and tag transfer from source to output.

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| **Vtx**   | Points | Dual graph vertices (one per original edge)          |
| **Edges** | Points | Dual graph edges connecting sequential edge-vertices |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDiagrams/Public/Elements/PCGExBuildDualGraph.h)
