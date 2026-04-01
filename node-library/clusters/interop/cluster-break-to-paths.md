---
description: Create individual paths from continuous edge chains.
icon: circle
---

# Cluster : Break to Paths

### Overview

This node extracts path point collections from cluster graphs by finding continuous edge chains. It identifies sequences of edges that form paths (nodes with exactly two neighbors) and outputs each chain as a separate point collection. This is the primary way to convert cluster topology back into path-based data for use with path nodes.

### How It Works

1. **Chain Detection**: Identifies continuous sequences of edges where nodes have exactly two connections
2. **Break Point Handling**: Uses optional filters to define custom break points within chains
3. **Direction Resolution**: Orders points within each path according to direction settings
4. **Winding Enforcement** (optional): Ensures consistent winding order for closed loops
5. **Output Filtering**: Applies min/max point count filters before outputting paths

**Usage Notes**

* **Leaves Handling**: Leaf nodes (endpoints with one connection) can be included, excluded, or processed exclusively.
* **Operation Target**: Choose between extracting full path chains or treating each edge as an individual path.
* **Break Conditions**: Connect cluster node filters to define additional points where chains should be broken.
* **Winding**: When enabled, projects paths to 2D and enforces clockwise or counter-clockwise ordering.

### Inputs

| Pin                  | Type                 | Description                             |
| -------------------- | -------------------- | --------------------------------------- |
| **Vtx**              | Points               | Cluster vertex points                   |
| **Edges**            | Points               | Cluster edge data                       |
| **Break Conditions** | Cluster Node Filters | Optional filters to define break points |

### Settings

#### Chain Extraction

<details>

<summary><strong>Leaves Handling</strong> <code>EPCGExBreakClusterLeavesHandling</code></summary>

How to handle leaf nodes (endpoints with only one connection).

| Option             | Description                           |
| ------------------ | ------------------------------------- |
| **Include Leaves** | Include leaves in the extracted paths |
| **Exclude Leaves** | Exclude leaves from paths             |
| **Only Leaves**    | Only process leaf nodes               |

Default: `Include Leaves`

</details>

<details>

<summary><strong>Operate On</strong> <code>EPCGExBreakClusterOperationTarget</code></summary>

What to extract from the cluster.

| Option    | Description                                                  |
| --------- | ------------------------------------------------------------ |
| **Paths** | Extract edge chains forming paths (nodes with two neighbors) |
| **Edges** | Treat each edge as an individual path (expensive)            |

Default: `Paths`

</details>

#### Direction Settings

<details>

<summary><strong>Direction Settings</strong> <code>FPCGExEdgeDirectionSettings</code></summary>

Controls the ordering of points within each extracted path.

//→ See TODO FPCGExEdgeDirectionSettings

</details>

#### Winding

<details>

<summary><strong>Winding</strong> <code>EPCGExWindingMutation</code></summary>

Enforce a winding order for paths.

| Option               | Description                     |
| -------------------- | ------------------------------- |
| **Unchanged**        | Keep original point order       |
| **Clockwise**        | Force clockwise winding         |
| **CounterClockwise** | Force counter-clockwise winding |

Default: `CounterClockwise`

</details>

<details>

<summary><strong>Wind Only Closed Loops</strong> <code>bool</code></summary>

Whether to apply winding only to closed loops or to all paths.

Default: `true`

</details>

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Projection settings for computing winding. Winding is computed on a 2D plane.

_Visible when Winding is not Unchanged_

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

#### Output Filtering

<details>

<summary><strong>Min Point Count</strong> <code>int32</code></summary>

Do not output paths that have fewer points than this value.

Default: `2`

</details>

<details>

<summary><strong>Omit Above Point Count</strong> <code>bool</code></summary>

Enable maximum point count filtering for output paths.

Default: `false`

</details>

<details>

<summary><strong>Max Point Count</strong> <code>int32</code></summary>

Do not output paths that have more points than this value.

Default: `500`

_Visible when Omit Above Point Count = true_

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| **Paths** | Points | Individual path point collections |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Paths/PCGExBreakClustersToPaths.h)
