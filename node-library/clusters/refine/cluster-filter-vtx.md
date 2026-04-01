---
description: Filter out vtx from clusters.
icon: circle-nodes
---

# Cluster : Filter Vtx

### Overview

This node filters vertices from cluster graphs based on connected filter conditions. It can output the result as modified clusters (with vertices and connected edges removed), as separate point collections (inside/outside), or simply write the filter result to an attribute.

### How It Works

1. **Filter Evaluation**: Evaluates connected node filters on each vertex
2. **Edge Handling** (optional): Invalidates edges connected to filtered vertices
3. **Output Generation**: Produces results based on the selected output mode
4. **Tagging** (optional): Tags output clusters based on filter pass/fail counts

**Usage Notes**

* **Node Filters Required**: Connect cluster node filters to define what passes/fails.
* **Edge Invalidation**: Enable to automatically remove edges connected to filtered vertices.
* **Output Modes**: Choose between cluster output, separate point outputs, or attribute-only.

### Inputs

| Pin                  | Type                 | Description                                   |
| -------------------- | -------------------- | --------------------------------------------- |
| **Vtx**              | Points               | Cluster vertex points                         |
| **Edges**            | Points               | Cluster edge data                             |
| **Node Filters**     | Cluster Node Filters | Filters to evaluate on vertices               |
| **Sanitize Filters** | Point Filters        | Optional edge filters for cluster output mode |

### Settings

#### Output Mode

<details>

<summary><strong>Mode</strong> <code>EPCGExVtxFilterOutput</code></summary>

Type of output to generate.

| Option        | Description                                                |
| ------------- | ---------------------------------------------------------- |
| **Clusters**  | Output filtered clusters with vertices/edges removed       |
| **Points**    | Output separate point collections (Inside/Outside)         |
| **Attribute** | Write filter result as an attribute, no structural changes |

Default: `Clusters`

</details>

<details>

<summary><strong>Result</strong> <code>FPCGExFilterResultDetails</code></summary>

Configuration for how filter results are written.

* **Action**: Write as bool, increment counter, or bitmask operation
* **Result Attribute Name**: Name of the output attribute

_Visible when Mode = Attribute_

</details>

#### Filter Behavior

<details>

<summary><strong>Node Invalidate Edges</strong> <code>bool</code></summary>

When a vertex is filtered out, also remove its connected edges.

Default: `false`

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the filter result - keep what would be removed, remove what would be kept.

Default: `false`

</details>

<details>

<summary><strong>Invert Edge Filters</strong> <code>bool</code></summary>

Invert the edge filter results separately.

Default: `false`

_Visible when Mode = Clusters_

</details>

#### Points Output Options

<details>

<summary><strong>Split Outputs By Connectivity</strong> <code>bool</code></summary>

Partition inside/outside groups by their original edge connectivity.

Default: `true`

_Visible when Mode = Points_

</details>

<details>

<summary><strong>Swap</strong> <code>bool</code></summary>

Swap Inside and Outside content.

Default: `false`

_Visible when Mode = Points_

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

| Pin         | Type   | Description                                         |
| ----------- | ------ | --------------------------------------------------- |
| **Vtx**     | Points | Filtered cluster vertices (Clusters/Attribute mode) |
| **Edges**   | Points | Filtered cluster edges (Clusters/Attribute mode)    |
| **Inside**  | Points | Points that passed the filter (Points mode)         |
| **Outside** | Points | Points that failed the filter (Points mode)         |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExFilterVtx.h)
