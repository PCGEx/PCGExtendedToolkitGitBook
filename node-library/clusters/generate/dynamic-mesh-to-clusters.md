---
description: Creates clusters from dynamic mesh topology.
icon: share-nodes
---

# Dynamic Mesh to Clusters

### Overview

Dynamic Mesh to Clusters converts PCG Dynamic Mesh data into cluster vertex and edge pairs. It extracts the mesh's triangle topology, merges split vertices (from hard edges or UV seams), and builds a cluster graph from the result. Several triangulation modes let you control whether the output represents raw triangle edges, a dual graph (face-to-face connectivity), boundary-only edges, or a hollow dual.

### How It Works

1. **Mesh Extraction**: Each input dynamic mesh is read and its vertices are merged using a spatial hash — split vertices that share the same position (within tolerance) are collapsed into one.
2. **Triangulation**: Depending on the selected mode, the mesh topology is processed as raw triangle edges, converted to a dual graph, reduced to boundary edges only, or transformed into a hollow dual.
3. **Point Generation**: Vertex positions are written to output points. Optionally, vertex colors and UV channel data are imported from the mesh onto the generated points.
4. **Cluster Building**: Edges are compiled into a cluster graph and output as standard Vtx/Edges pairs. Input tags from each mesh are forwarded to the corresponding output data.

**Usage Notes**

* **Vertex merging**: Meshes often have split vertices at hard edges and UV seams. The merge tolerance controls how aggressively these are collapsed. Too low and you get duplicate vertices; too high and distinct vertices may merge.
* **Boundaries mode**: Only outputs edges along the mesh hull — useful for extracting silhouettes or outlines from a mesh.
* **Main thread only**: This node executes on the main thread due to dynamic mesh API requirements.
* **Tag forwarding**: Tags from each input mesh entry are forwarded to the corresponding output vertex data, preserving metadata through the pipeline.

### Behavior

```
Dynamic Mesh Input          Output (Raw mode)
┌──────────────┐           ┌──────────────┐
│  ╱\  ╱\  ╱\  │    →      │ Vtx: mesh vertices (merged)
│ ╱__\╱__\╱__\ │    →      │ Edges: triangle edges
└──────────────┘           └──────────────┘

                           Output (Dual mode)
                           ┌──────────────┐
                    →      │ Vtx: triangle centers
                    →      │ Edges: face-to-face adjacency
                           └──────────────┘
```

### Inputs

| Pin              | Type         | Description                         |
| ---------------- | ------------ | ----------------------------------- |
| **Dynamic Mesh** | Dynamic Mesh | One or more PCG Dynamic Mesh inputs |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Graph Output Type</strong> <code>EPCGExTriangulationType</code></summary>

Controls how the mesh topology is converted into cluster edges.

| Option         | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| **Raw**        | Direct triangle edges — each mesh edge becomes a cluster edge           |
| **Dual**       | Dual graph — vertices at triangle centers, edges between adjacent faces |
| **Hollow**     | Hollow dual graph — like Dual but excludes interior structure           |
| **Boundaries** | Boundary edges only — outputs only edges along the mesh hull/silhouette |

Default: `Raw`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Import Details</strong> <code>FPCGExGeoMeshImportDetails</code></summary>

Controls which data is imported from the dynamic mesh onto the generated points, such as vertex colors and UV channels.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore Mesh Warnings</strong> <code>bool</code></summary>

Skip invalid meshes and suppress warning messages about them.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Vertex Merge Hash Tolerance</strong> <code>float</code></summary>

Tolerance for merging vertices that share the same position, such as those found at split vertices along hard edges or UV seams. Higher values merge more aggressively; the value is clamped to a small positive minimum to prevent division by zero.

Default: `PCGExMesh::DefaultVertexMergeHashTolerance`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Precise Vertex Merge</strong> <code>bool</code></summary>

Uses two overlapping spatial hashes to detect vertex proximity. More accurate but slightly slower and uses more memory during processing.

Default: `false`

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Graph and edge output properties — controls how the final cluster data is compiled and written.

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| **Vtx**   | Points | Cluster vertices generated from mesh topology |
| **Edges** | Points | Cluster edges connecting the vertices         |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTopology-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTopology/Public/Elements/PCGExDynamicMeshToClusters.h)
