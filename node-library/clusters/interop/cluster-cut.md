---
description: Cut clusters nodes & edges using paths.
icon: share-nodes
---

# Cluster : Cut

### Overview

This node removes or preserves cluster elements based on their intersection with input paths. It can detect overlaps between paths and cluster nodes, edges, or both, allowing you to carve shapes out of clusters or isolate regions that intersect with specific paths.

### How It Works

1. **Path Processing**: Analyzes input path collections for intersection testing
2. **Intersection Detection**: Tests cluster elements against paths based on the selected mode
3. **Propagation** (optional): Affected nodes can affect connected edges and vice versa
4. **Graph Rebuild**: Outputs a modified cluster with affected elements removed or preserved

**Usage Notes**

* **Mode Selection**: Choose whether to test nodes, edges, or both for path intersection.
* **Invert Mode**: When enabled, keeps intersecting elements instead of removing them.
* **Node Expansion**: Expand node bounds for more generous overlap detection.
* **Filters**: Use Node and Edge Filters to further refine which elements can be affected.

### Inputs

| Pin             | Type          | Description                                 |
| --------------- | ------------- | ------------------------------------------- |
| **Vtx**         | Points        | Cluster vertex points                       |
| **Edges**       | Points        | Cluster edge data                           |
| **Paths**       | Points        | Path collections to cut with                |
| **NodeFilters** | Point Filters | Optional filters for which nodes can be cut |
| **EdgeFilters** | Point Filters | Optional filters for which edges can be cut |

### Settings

#### Intersection

<details>

<summary><strong>Intersection Details</strong> <code>FPCGExPathEdgeIntersectionDetails</code></summary>

Settings for detecting path-to-edge intersections.

//→ See TODO FPCGExPathEdgeIntersectionDetails

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, keep intersecting elements instead of removing them.

Default: `false`

</details>

<details>

<summary><strong>Mode</strong> <code>EPCGExCutEdgesMode</code></summary>

What cluster elements to check for path overlap.

| Option            | Description                                 |
| ----------------- | ------------------------------------------- |
| **Nodes**         | Check for path overlap with nodes only      |
| **Edges**         | Check for path overlap with edges only      |
| **Edges & Nodes** | Check for overlap with both nodes and edges |

Default: `Edges & Nodes`

</details>

#### Node Settings

<details>

<summary><strong>Node Expansion</strong> <code>double</code></summary>

Expansion factor for node bounds when checking for overlap. Uses scaled bounds expanded by this value.

Default: `1`

_Visible when Mode includes Nodes_

</details>

<details>

<summary><strong>Node Distance Settings</strong> <code>EPCGExDistance</code></summary>

How to measure distance from path to node.

| Option            | Description                    |
| ----------------- | ------------------------------ |
| **Center**        | Measure from node center point |
| **Sphere Bounds** | Use spherical bounds           |
| **Box Bounds**    | Use box bounds                 |

Default: `Center`

_Visible when Mode includes Nodes_

</details>

#### Propagation

<details>

<summary><strong>Affected Nodes Affect Connected Edges</strong> <code>bool</code></summary>

When a node is affected, also mark its connected edges as affected.

Default: `false`

_Visible when Mode includes Nodes_

</details>

<details>

<summary><strong>Affected Edges Affect Endpoints</strong> <code>bool</code></summary>

When an edge is affected, also mark its endpoint nodes as affected.

Default: `false`

_Visible when not Inverted and Mode includes Edges_

</details>

<details>

<summary><strong>Keep Edges That Connect Valid Nodes</strong> <code>bool</code></summary>

When inverting, keep edges that connect two preserved nodes even if the edge itself wasn't preserved.

Default: `false`

_Visible when Inverted and Mode includes Nodes_

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

| Pin       | Type   | Description               |
| --------- | ------ | ------------------------- |
| **Vtx**   | Points | Modified cluster vertices |
| **Edges** | Points | Modified cluster edges    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Paths/PCGExCutClusters.h)
