---
description: Extract & write extra edge information to the point representing the edge.
icon: share-nodes
---

# Cluster : Edge Properties

### Overview

This node computes and writes various properties to edge points in a cluster, including edge length, direction, heuristic scores, and solidification data. It can also blend attributes from endpoint vertices onto edges, making it essential for leveraging edges in complex cluster operations.

### How It Works

1. **Direction Resolution**: Determines edge direction based on the selected method (endpoint order, attribute, etc.)
2. **Property Computation**: Calculates requested properties like length, direction vector, and heuristics
3. **Endpoint Blending** (optional): Blends vertex attributes from both endpoints onto the edge point
4. **Solidification** (optional): Positions and orients edge points for mesh/spline generation with radius data

**Usage Notes**

* **Edge Direction**: Many outputs depend on edge direction - configure the Direction Settings to control how start/end are determined.
* **Solidification**: Enable solidification axis to prepare edges for spline or mesh generation, with configurable radii per axis.
* **Heuristics**: Connect heuristic nodes to compute pathfinding-style scores for edges.

{% hint style="info" %}
### Heuristics Limitations

Not all heuristics will produce meaningful scores in this context. Some heuristics are designed for pathfinding and rely on:

* **Seed/Goal context** - knowing the start and end points of a path
* **Travel stack history** - tracking the traversal path taken to reach an edge

Without this context, these heuristics may return default or nonsensical values. Heuristics that work well here are those based purely on local vtx & edges properties (like steepness or edge length).
{% endhint %}

### Inputs

| Pin            | Type                 | Description                                    |
| -------------- | -------------------- | ---------------------------------------------- |
| **Vtx**        | Points               | Cluster vertex points                          |
| **Edges**      | Points               | Cluster edge data                              |
| **Heuristics** | Heuristics Factories | Optional heuristic nodes for score computation |

### Settings

#### Direction Settings

<details>

<summary><strong>Direction Method</strong> <code>EPCGExEdgeDirectionMethod</code></summary>

Method used to determine edge direction (which endpoint is "start" vs "end").

| Option                 | Description                                   |
| ---------------------- | --------------------------------------------- |
| **EndpointsOrder**     | Use the order endpoints appear in edge data   |
| **EndpointsAttribute** | Use an attribute value to determine direction |
| **EndpointsSort**      | Use sorting rules to determine direction      |

Default: `EndpointsOrder`

</details>

<details>

<summary><strong>Direction Choice</strong> <code>EPCGExEdgeDirectionChoice</code></summary>

When using attribute-based direction, whether to go from smallest-to-greatest or greatest-to-smallest.

Default: `SmallestToGreatest`

</details>

#### Outputs

<details>

<summary><strong>Write Edge Length</strong> <code>bool</code></summary>

Output the edge length as an attribute.

Default: `false`

</details>

<details>

<summary><strong>Edge Length</strong> <code>FName</code></summary>

Attribute name for edge length.

Default: `EdgeLength`

_Visible when Write Edge Length = true_

</details>

<details>

<summary><strong>Write Edge Direction</strong> <code>bool</code></summary>

Output the edge direction as a vector attribute.

Default: `false`

</details>

<details>

<summary><strong>Edge Direction</strong> <code>FName</code></summary>

Attribute name for edge direction vector.

Default: `EdgeDirection`

_Visible when Write Edge Direction = true_

</details>

<details>

<summary><strong>Endpoints Blending</strong> <code>bool</code></summary>

Blend attributes from endpoint vertices onto the edge point.

Default: `false`

</details>

<details>

<summary><strong>Endpoints Weights</strong> <code>double</code></summary>

Balance between start (0) and end (1) point for blending.

Default: `0.5`

_Visible when Endpoints Blending = true_

</details>

<details>

<summary><strong>Write Heuristics</strong> <code>bool</code></summary>

Compute and write heuristic scores to edges.

Default: `false`

</details>

<details>

<summary><strong>Heuristics</strong> <code>FName</code></summary>

Attribute name for heuristic score.

Default: `Heuristics`

_Visible when Write Heuristics = true_

</details>

<details>

<summary><strong>Heuristics Mode</strong> <code>EPCGExHeuristicsWriteMode</code></summary>

How to compute heuristics when direction matters.

| Option             | Description                                |
| ------------------ | ------------------------------------------ |
| **EndpointsOrder** | Use endpoint order for heuristic direction |
| **Smallest**       | Compute both ways, keep smallest score     |
| **Highest**        | Compute both ways, keep highest score      |

Default: `EndpointsOrder`

_Visible when Write Heuristics = true_

</details>

#### Solidification

<details>

<summary><strong>Write Edge Position</strong> <code>bool</code></summary>

Update edge point position as a lerp between endpoints.

Default: `false`

</details>

<details>

<summary><strong>Edge Position Lerp</strong> <code>double</code></summary>

Position along edge (0 = start, 0.5 = middle, 1 = end).

Default: `0.5`

_Visible when Write Edge Position = true_

</details>

<details>

<summary><strong>Solidification Axis</strong> <code>EPCGExMinimalAxis</code></summary>

Align edge point rotation to edge direction along this axis. Required for radius outputs.

| Option   | Description                    |
| -------- | ------------------------------ |
| **None** | No solidification              |
| **X**    | Align X axis to edge direction |
| **Y**    | Align Y axis to edge direction |
| **Z**    | Align Z axis to edge direction |

Default: `None`

</details>

<details>

<summary><strong>Write Radius X/Y/Z</strong> <code>bool</code></summary>

Write edge radius values for each axis (excluding the solidification axis). Can be constant or from attributes.

Default: `false`

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| **Vtx**   | Points | Unchanged vertex data          |
| **Edges** | Points | Edges with computed properties |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/PCGExWriteEdgeProperties.h)
