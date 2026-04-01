---
description: Base classes for filters that operate on graph clusters (vertices and edges).
icon: code
---

# Cluster Filter Definition

### Overview

This is the abstract base class hierarchy that defines the structure for cluster-aware filters in PCGExtended. Unlike point filters that operate on individual points, cluster filters have access to the full graph topology, allowing them to evaluate vertices based on their connections or edges based on their endpoints. The hierarchy provides two specializations: vertex (node) filters and edge filters.

### How It Works

1. **Inheritance**: Cluster filters extend point filters, gaining access to cluster topology in addition to point data.
2. **Initialization**: When used with a cluster, filters receive references to both the vertex data facade and the edge data facade.
3. **Evaluation**: Vertex filters test nodes within their graph context; edge filters test edges based on their connected vertices.
4. **Type Dispatch**: The system automatically routes filter evaluation to the appropriate method based on whether a vertex or edge is being tested.

**Usage Notes**

* **Cluster Context Required**: These filters only function when initialized with cluster data. Using them on plain point data will skip the cluster-specific logic.
* **Collection Evaluation**: Cluster filters do not support collection-level evaluation (`SupportsCollectionEvaluation` returns false) since they require per-cluster initialization.
* **Dual Data Access**: Edge filters can access attributes from both the edge data and the vertex data of their endpoints.

### Class Hierarchy

```
UPCGExFilterFactoryData (base filter)
  └── UPCGExPointFilterFactoryData (point filters)
        └── UPCGExClusterFilterFactoryData (cluster-aware filters)
              ├── UPCGExNodeFilterFactoryData (vertex/node filters)
              └── UPCGExEdgeFilterFactoryData (edge filters)
```

### Filter Types

#### Vertex (Node) Filters

Vertex filters evaluate cluster nodes based on their properties and connections.

| Data Type                         | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| **PCGEx \| Filter (Cluster Vtx)** | Filters that test individual vertices within a cluster graph |

**Provider Settings**: `UPCGExVtxFilterProviderSettings`

Vertex filters receive the full node context, including:

* Node position and attributes
* Connected edges
* Adjacent node references

#### Edge Filters

Edge filters evaluate connections between vertices.

| Data Type                           | Description                                                 |
| ----------------------------------- | ----------------------------------------------------------- |
| **PCGEx \| Filter (Cluster Edges)** | Filters that test edges between vertices in a cluster graph |

**Provider Settings**: `UPCGExEdgeFilterProviderSettings`

Edge filters receive the full edge context, including:

* Edge attributes
* Start and end vertex references
* Edge geometry (length, direction)

### Inherited Settings

Cluster filters inherit all settings from point filters.

> See Filter Definition for base filter settings.

### Outputs

| Pin             | Type                            | Description                                                    |
| --------------- | ------------------------------- | -------------------------------------------------------------- |
| **Vtx Filter**  | PCGEx \| Filter (Cluster Vtx)   | Outputs a vertex filter factory (from vertex filter providers) |
| **Edge Filter** | PCGEx \| Filter (Cluster Edges) | Outputs an edge filter factory (from edge filter providers)    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Core/PCGExClusterFilter.h)
