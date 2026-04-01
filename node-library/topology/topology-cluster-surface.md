---
description: Create a cluster surface topology.
icon: circle
---

# Topology : Cluster Surface

### Overview

This node converts point clusters with edge connections into triangulated surface meshes. Each cluster's cells (the polygons formed by connected edges) are identified, filtered by constraints, and triangulated into a PCG Dynamic Mesh output.

### How It Works

1. **Identify Cells**: The node traces the cluster's edge connectivity to identify closed cells (polygons formed by the graph structure).
2. **Project to 2D**: Points are projected onto a 2D plane for triangulation using the specified projection method.
3. **Apply Constraints**: Cells are filtered based on size, point count, area, perimeter, compactness, and other geometric properties.
4. **Triangulate**: Valid cells are triangulated to create mesh faces.
5. **Build Mesh**: All triangles are combined into a single PCG Dynamic Mesh with optional UV mapping, normals, and material assignment.

**Usage Notes**

* **Cluster Input**: This node expects cluster data (vertices + edges) as input. Use cluster-building nodes upstream to create the graph structure.
* **Cell Detection**: A "cell" is any closed polygon formed by following edges around the cluster. The algorithm finds all minimal cycles in the graph.
* **Edge Constraints**: You can optionally filter which edges participate in cell formation using the edge constraints input pin.

### Behavior

```
Input: Point cluster          Output: Triangulated mesh
with edges
    ●───●───●                     ●───●───●
    │ ╲ │ ╱ │                     │\  │  /│
    │  ╲│╱  │         →           │ \ │ / │
    ●───●───●                     ●──\●/──●
    │ ╱ │ ╲ │                     │  /│\  │
    ●───●───●                     ●───●───●

Each enclosed region becomes a triangulated face in the output mesh.
```

### Inputs

| Pin                  | Type             | Description                                          |
| -------------------- | ---------------- | ---------------------------------------------------- |
| **Vtx**              | Points           | Cluster vertices                                     |
| **Edges**            | Points           | Cluster edge data                                    |
| **Edge Constraints** | Filter Factories | Optional filters to constrain which edges form cells |

### Outputs

| Pin      | Type             | Description                   |
| -------- | ---------------- | ----------------------------- |
| **Mesh** | PCG Dynamic Mesh | The triangulated surface mesh |

### Settings

This node uses all settings from its base class with no additional node-specific settings.

#### Inherited Settings

→ See Topology Clusters Processor for all settings including:

* **Output Mode** - PCG Dynamic Mesh output format
* **Projection Details** - 2D projection settings for triangulation
* **Cell Constraints** - Filtering cells by size, area, perimeter, compactness, etc.
* **Topology Details** - Material, UV channels, normals, and mesh generation options

→ See Clusters Processor Settings for: Cluster handling and point data options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTopology-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTopology/Public/Elements/PCGExTopologyClusterSurface.h)
