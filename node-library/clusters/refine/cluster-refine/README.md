---
description: Refine edges according to special rules.
icon: circle-nodes
---

# Cluster : Refine

### Overview

This node applies refinement operations to cluster edges, allowing you to prune, filter, or transform the graph topology based on various criteria. Multiple refinement strategies are available, from geometric algorithms like Gabriel graph refinement to score-based and filter-based edge selection.

### How It Works

1. **Operation Selection**: Choose a refinement operation (or connect one via override pin)
2. **Edge Evaluation**: Each edge is evaluated according to the refinement rules
3. **Sanitization** (optional): Restore edges for nodes that would become disconnected
4. **Output Generation**: Produces refined clusters, points, or attributes

**Usage Notes**

* **Refinement Selection**: Select the refinement type in the node details or connect via the Overrides pin.
* **Heuristics Support**: Many refinements can use heuristic scores for decision making.
* **Sanitization**: Enable to prevent nodes from losing all connections.

### Available Refinements

| Refinement               | Description                                                          |
| ------------------------ | -------------------------------------------------------------------- |
| **By Filter**            | Remove edges based on filter conditions                              |
| **Gabriel**              | Gabriel graph refinement (removes edges with points in circumcircle) |
| **Keep Highest Score**   | Keep only the highest-scoring edge per node                          |
| **Keep Longest**         | Keep only the longest edge per node                                  |
| **Keep Lowest Score**    | Keep only the lowest-scoring edge per node                           |
| **Keep Shortest**        | Keep only the shortest edge per node                                 |
| **Line Trace**           | Remove edges blocked by collision                                    |
| **Remove Highest Score** | Remove the highest-scoring edge per node                             |
| **Remove Leaves**        | Remove leaf edges (endpoints with single connection)                 |
| **Remove Longest**       | Remove the longest edge per node                                     |
| **Remove Lowest Score**  | Remove the lowest-scoring edge per node                              |
| **Remove Shortest**      | Remove the shortest edge per node                                    |
| **Skeleton**             | Compute minimum spanning tree / skeleton                             |

### Inputs

| Pin                        | Type                 | Description                                     |
| -------------------------- | -------------------- | ----------------------------------------------- |
| **Vtx**                    | Points               | Cluster vertex points                           |
| **Edges**                  | Points               | Cluster edge data                               |
| **Heuristics**             | Heuristics Factories | Optional heuristics for score-based refinements |
| **Sanitize Filters**       | Point Filters        | Optional filters for sanitization mode          |
| **Overrides : Refinement** | Refinement Factory   | Override the built-in refinement selection      |

### Settings

#### Refinement

<details>

<summary><strong>Refinement</strong> <code>UPCGExEdgeRefineInstancedFactory</code></summary>

The refinement operation to apply. Select from the available options in the dropdown.

</details>

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

How to combine multiple heuristic scores when using score-based refinements.

Default: `WeightedAverage`

</details>

#### Output Mode

<details>

<summary><strong>Mode</strong> <code>EPCGExRefineEdgesOutput</code></summary>

Type of output to generate.

| Option        | Description                                    |
| ------------- | ---------------------------------------------- |
| **Clusters**  | Output refined clusters with edges removed     |
| **Points**    | Output kept/removed edges as point collections |
| **Attribute** | Write refinement results as attributes         |

Default: `Clusters`

</details>

<details>

<summary><strong>Vtx Result</strong> <code>FPCGExFilterResultDetails</code></summary>

Configuration for writing vertex refinement results.

_Visible when Mode = Attribute_

</details>

<details>

<summary><strong>Edge Result</strong> <code>FPCGExFilterResultDetails</code></summary>

Configuration for writing edge refinement results.

_Visible when Mode = Attribute_

</details>

#### Sanitization

<details>

<summary><strong>Sanitization</strong> <code>EPCGExRefineSanitization</code></summary>

How to handle nodes that would lose all edges after refinement.

| Option       | Description                                      |
| ------------ | ------------------------------------------------ |
| **None**     | No sanitization, allow disconnected nodes        |
| **Shortest** | Restore the shortest edge for disconnected nodes |
| **Longest**  | Restore the longest edge for disconnected nodes  |
| **Filters**  | Use filters to determine which edges to preserve |

Default: `None`

_Visible when Mode = Clusters_

</details>

<details>

<summary><strong>Restore Edges That Connect To Valid Nodes</strong> <code>bool</code></summary>

Restore edges if they would connect to nodes that still have valid connections.

Default: `false`

_Visible when Mode = Clusters_

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

| Pin               | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| **Vtx**           | Points | Refined cluster vertices                   |
| **Edges**         | Points | Refined cluster edges                      |
| **Kept Edges**    | Points | Edges that passed refinement (Points mode) |
| **Removed Edges** | Points | Edges that failed refinement (Points mode) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExRefineEdges.h)
