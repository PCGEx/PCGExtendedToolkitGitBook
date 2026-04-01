---
description: >-
  Ensure the input set of vertex and edges outputs clean, interconnected
  clusters. May create new clusters, but does not create nor delete
  points/edges.
icon: share-nodes
---

# Cluster : Sanitize

### Overview

This node validates and reorganizes cluster data to ensure proper connectivity. It analyzes the input vertices and edges, identifies disconnected sub-graphs, and outputs them as separate clean clusters. The node ensures that each output cluster is a fully connected component where all vertices are reachable from any other vertex through edges.

### How It Works

1. **Analyze Connectivity**: Examines all vertices and edges to identify which vertices are connected to each other.
2. **Identify Components**: Finds all disconnected sub-graphs (connected components) within the input data.
3. **Rebuild Clusters**: Outputs each connected component as a separate, valid cluster.
4. **Apply Filters**: Optionally removes clusters that are too small or too large based on vertex/edge count thresholds.

**Usage Notes**

* **No Point Deletion**: This node never deletes points or edges - it only reorganizes them into proper clusters.
* **Cluster Splitting**: A single input edge set may produce multiple output clusters if the graph contains disconnected components.
* **Isolated Points**: Isolated points (vertices with no edges) are handled according to the inherited cluster processor settings.

### Settings

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Inherited Settings

This node inherits common cluster processing settings from its base class.

> See Clusters Processor Settings for: Cluster handling, edge validation, and other common cluster configuration.

### Inputs

| Pin       | Type   | Description      |
| --------- | ------ | ---------------- |
| **Vtx**   | Points | Cluster vertices |
| **Edges** | Points | Cluster edges    |

### Outputs

| Pin       | Type   | Description                                                                                |
| --------- | ------ | ------------------------------------------------------------------------------------------ |
| **Vtx**   | Points | Sanitized cluster vertices                                                                 |
| **Edges** | Points | Sanitized cluster edges (may be split into multiple edge sets for disconnected components) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExSanitizeClusters.h)
