---
description: >-
  Abstract base class for vertex property sub-nodes that compute and write
  per-vertex data based on cluster topology.
icon: arrow-down-from-arc
---

# Vtx Property Provider

### Overview

Vertex Property providers are sub-nodes that analyze the connectivity and adjacency relationships of vertices within a cluster, then write computed values as attributes. Each provider operates on vertices and their edges, computing properties like directions to neighbors, edge lengths, or topological metrics.

This is the base class that all specific vertex property implementations inherit from. The concrete implementations define what specific data gets computed and written.

### How It Works

1. **Cluster Preparation**: The provider receives the cluster data including vertex positions and edge connectivity.
2. **Per-Node Processing**: For each vertex in the cluster, the provider examines its adjacency data (connected edges and neighbor vertices).
3. **Attribute Writing**: Computed values are written to the vertex data as new attributes based on the provider's configuration.

### Common Output Patterns

Vertex property providers commonly use these output structures:

#### Simple Edge Output Settings

Basic edge data output for direction and length:

| Output        | Type      | Description                                                 |
| ------------- | --------- | ----------------------------------------------------------- |
| **Direction** | `FVector` | Direction vector from this vertex toward a connected vertex |
| **Length**    | `double`  | Distance to the connected vertex                            |

#### Extended Edge Output Settings

Adds index tracking to the simple outputs:

| Output             | Type      | Description                               |
| ------------------ | --------- | ----------------------------------------- |
| **Direction**      | `FVector` | Direction vector to connected vertex      |
| **Length**         | `double`  | Distance to connected vertex              |
| **Edge Index**     | `int32`   | Index of the connecting edge              |
| **Vtx Index**      | `int32`   | Index of the connected vertex             |
| **Neighbor Count** | `int32`   | Total number of neighbors for this vertex |

### Settings

This abstract base class has no configurable settings. See the specific vertex property implementations for their settings:

* Vtx : Special Edges
* Vtx : Special Neighbors

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/VtxProperties/PCGExVtxPropertyFactoryProvider.h)
