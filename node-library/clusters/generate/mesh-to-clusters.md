---
description: Creates clusters from mesh topology.
icon: share-nodes
---

# Mesh to Clusters

### Overview

This node converts static mesh geometry into cluster graphs. It extracts the vertex positions and edge connectivity from mesh triangles, outputting them as PCGEx cluster data. Multiple output types are available, from raw triangle edges to dual graphs and boundary extraction.

### How It Works

1. **Mesh Loading**: Loads static mesh data from constant reference or point attribute
2. **Vertex Extraction**: Extracts unique vertex positions with optional merging of split vertices
3. **Topology Conversion**: Converts mesh faces to graph edges based on the selected output type
4. **Transform Application**: Applies target point transforms to the generated clusters
5. **Data Import** (optional): Imports vertex colors and UVs from the mesh

**Usage Notes**

* **Graph Types**: Choose between raw triangles, dual graph, hollow, or boundary-only outputs.
* **Vertex Merging**: Adjust tolerance to merge vertices at UV seams or hard edges.
* **Per-Point Meshes**: Use attribute mode to generate different mesh clusters per input point.

### Inputs

| Pin         | Type   | Description                                         |
| ----------- | ------ | --------------------------------------------------- |
| **Targets** | Points | Target points where mesh clusters will be generated |

### Settings

#### Graph Output

<details>

<summary><strong>Graph Output Type</strong> <code>EPCGExTriangulationType</code></summary>

Type of graph structure to generate from the mesh.

| Option              | Description                              |
| ------------------- | ---------------------------------------- |
| **Raw Triangles**   | Direct triangle edges from mesh topology |
| **Dual Graph**      | Graph connecting triangle centroids      |
| **Hollow Graph**    | Edges without interior triangulation     |
| **Boundaries**      | Only boundary/silhouette edges           |
| **NoTriangulation** | Vertices only, no edges                  |

Default: `Raw Triangles`

</details>

#### Mesh Source

<details>

<summary><strong>Static Mesh Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the static mesh reference.

| Option        | Description                              |
| ------------- | ---------------------------------------- |
| **Constant**  | Use a fixed static mesh asset            |
| **Attribute** | Read mesh reference from point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Static Mesh</strong> <code>TSoftObjectPtr&#x3C;UStaticMesh></code></summary>

The static mesh asset to convert.

_Visible when Static Mesh Input = Constant_

</details>

<details>

<summary><strong>Attribute Handling</strong> <code>EPCGExMeshAttributeHandling</code></summary>

How to interpret the mesh attribute.

| Option                   | Description                                                      |
| ------------------------ | ---------------------------------------------------------------- |
| **StaticMesh Soft Path** | Attribute contains a soft object path to a static mesh           |
| **Actor Reference**      | Attribute contains an actor reference to extract primitives from |

_Visible when Static Mesh Input = Attribute_

</details>

#### Transform

<details>

<summary><strong>Transform Details</strong> <code>FPCGExTransformDetails</code></summary>

How to transform the generated clusters based on target points.

//→ See TODO FPCGExTransformDetails

</details>

#### Mesh Import

<details>

<summary><strong>Import Details</strong> <code>FPCGExGeoMeshImportDetails</code></summary>

What data to import from the static mesh.

* **Import Vertex Color**: Copy vertex colors as point attributes
* **Import UVs**: Copy UV coordinates with configurable channel mapping

</details>

#### Vertex Merging

<details>

<summary><strong>Ignore Mesh Warnings</strong> <code>bool</code></summary>

Skip invalid meshes without throwing warnings.

Default: `false`

</details>

<details>

<summary><strong>Vertex Merge Hash Tolerance</strong> <code>float</code></summary>

Tolerance for merging vertices at split locations (UV seams, hard edges).

Default: `0.001`

</details>

<details>

<summary><strong>Precise Vertex Merge</strong> <code>bool</code></summary>

Use two overlapping spatial hashes for more accurate vertex merging. Slightly slower but more accurate.

Default: `false`

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Write edge midpoint position as an attribute on edge points.

//→ See TODO FPCGExGraphBuilderDetails

</details>

#### Attributes Forwarding

<details>

<summary><strong>Attributes Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Which input point attributes to forward onto the generated clusters.

//→ See TODO FPCGExForwardDetails

</details>

### Outputs

| Pin              | Type   | Description                      |
| ---------------- | ------ | -------------------------------- |
| **Vtx**          | Points | Cluster vertices from mesh       |
| **Edges**        | Points | Cluster edges from mesh topology |
| **BaseMeshData** | Points | Original mesh reference data     |

***

📦 [![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExMeshToClusters.h)
