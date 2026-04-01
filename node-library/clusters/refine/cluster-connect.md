---
description: >-
  Connects isolated edge clusters by their closest vertices, if they share the
  same vtx group.
icon: share-nodes
---

# Cluster : Connect

### Overview

This node creates bridge edges between separate clusters that share the same vertex point collection. It analyzes multiple disconnected clusters and creates new edges to connect them, effectively merging isolated graph components into a unified network.

### How It Works

1. **Cluster Analysis**: Identifies separate clusters within the same vertex group
2. **Bridge Computation**: Uses the selected method to find connection points between clusters
3. **Edge Creation**: Creates new bridge edges connecting the clusters
4. **Graph Rebuild**: Outputs a unified cluster with all bridges included

**Usage Notes**

* **Same Vertex Group**: Only clusters sharing the same Vtx point collection can be connected.
* **Method Selection**: Different methods produce different connection patterns - Delaunay creates organic connections, Least Edges minimizes bridges.
* **Bridge Flagging**: Enable output flags to identify which vertices and edges are bridge connections.

### Inputs

| Pin       | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| **Vtx**   | Points | Cluster vertex points                                    |
| **Edges** | Points | Multiple edge collections representing separate clusters |

### Settings

#### Connection Method

<details>

<summary><strong>Connect Method</strong> <code>EPCGExBridgeClusterMethod</code></summary>

Method used to find and insert bridge edges between clusters.

| Option          | Description                                           |
| --------------- | ----------------------------------------------------- |
| **Delaunay 3D** | Use 3D Delaunay triangulation to find connections     |
| **Delaunay 2D** | Use 2D Delaunay triangulation (requires projection)   |
| **Least Edges** | Create minimum bridges needed to connect all clusters |
| **Most Edges**  | Create bridges between every pair of clusters         |

Default: `Delaunay 3D`

</details>

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Projection settings for 2D Delaunay computation.

_Visible when Connect Method = Delaunay 2D_

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

#### Carry Over Settings

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are preserved from input to output.

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Additional Outputs

<details>

<summary><strong>Flag Vtx Connector</strong> <code>bool</code></summary>

Write the number of bridges connected to each vertex.

Default: `false`

</details>

<details>

<summary><strong>Vtx Connector Flag Name</strong> <code>FName</code></summary>

Attribute name for the vertex bridge count.

Default: `NumBridges`

_Visible when Flag Vtx Connector = true_

</details>

<details>

<summary><strong>Flag Edge Connector</strong> <code>bool</code></summary>

Mark edges that are bridges between clusters.

Default: `false`

</details>

<details>

<summary><strong>Edge Connector Flag Name</strong> <code>FName</code></summary>

Attribute name for the bridge edge flag.

Default: `IsBridge`

_Visible when Flag Edge Connector = true_

</details>

#### Warnings and Errors

<details>

<summary><strong>Quiet No Bridge Warning</strong> <code>bool</code></summary>

Suppress warnings when no bridges could be created.

Default: `false`

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| **Vtx**   | Points | Unified cluster vertices               |
| **Edges** | Points | Edges including new bridge connections |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExConnectClusters.h)
