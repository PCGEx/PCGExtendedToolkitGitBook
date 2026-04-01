---
description: Extract & write extra information from the edges connected to the vtx.
icon: circle-nodes
---

# Cluster : Vtx Properties

### Overview

This node computes and writes vertex properties derived from their edge connections, such as edge count and vertex normals. It can also transform vertices into oriented bounding boxes (OOB) based on their neighboring edge directions, useful for creating properly-oriented geometry at junction points.

### How It Works

1. **Edge Analysis**: For each vertex, analyzes its connected edges
2. **Property Computation**: Calculates requested properties (edge count, normal direction)
3. **OOB Transformation** (optional): Computes oriented bounding box from edge directions and applies to vertex transform
4. **Attribute Writing**: Writes computed values to vertex attributes

**Usage Notes**

* **Vertex Normal**: The normal is computed from the oriented bounding box of connected edge directions, aligned to the specified axis.
* **OOB Mutation**: When enabled, vertex transforms are modified to represent an oriented bounding box encompassing the edge directions.
* **Extra Properties**: Connect Vtx Property sub-nodes to compute additional custom properties.

### Inputs

| Pin            | Type                   | Description                                         |
| -------------- | ---------------------- | --------------------------------------------------- |
| **Vtx**        | Points                 | Cluster vertex points                               |
| **Edges**      | Points                 | Cluster edge data                                   |
| **Properties** | Vtx Property Factories | Optional property sub-nodes for custom computations |

### Settings

#### Outputs

<details>

<summary><strong>Mutate Vtx To OOB</strong> <code>bool</code></summary>

Transform vertex points into oriented bounding boxes based on their connected edge directions.

Default: `false`

</details>

<details>

<summary><strong>Precise Fit</strong> <code>bool</code></summary>

Use precise minimum box fit algorithm for OOB computation.

Default: `true`

_Visible when Mutate Vtx To OOB = true_

</details>

<details>

<summary><strong>Axis Order</strong> <code>EPCGExAxisOrder</code></summary>

Priority order for axis alignment when computing OOB.

| Option  | Description        |
| ------- | ------------------ |
| **XYZ** | X > Y > Z priority |
| **YZX** | Y > Z > X priority |
| **ZXY** | Z > X > Y priority |
| **YXZ** | Y > X > Z priority |
| **ZYX** | Z > Y > X priority |
| **XZY** | X > Z > Y priority |

Default: `XYZ`

_Visible when Mutate Vtx To OOB = true_

</details>

<details>

<summary><strong>Write Vtx Edge Count</strong> <code>bool</code></summary>

Write the number of edges connected to each vertex.

Default: `false`

</details>

<details>

<summary><strong>Edge Count</strong> <code>FName</code></summary>

Attribute name for edge count.

Default: `EdgeCount`

_Visible when Write Vtx Edge Count = true_

</details>

<details>

<summary><strong>Write Vtx Normal</strong> <code>bool</code></summary>

Write a normal vector computed from connected edge directions.

Default: `false`

</details>

<details>

<summary><strong>Normal</strong> <code>FName</code></summary>

Attribute name for normal vector.

Default: `Normal`

_Visible when Write Vtx Normal = true_

</details>

<details>

<summary><strong>Axis</strong> <code>EPCGExMinimalAxis</code></summary>

Which axis of the vertex OOB to use as the normal direction.

| Option | Description          |
| ------ | -------------------- |
| **X**  | Use X axis as normal |
| **Y**  | Use Y axis as normal |
| **Z**  | Use Z axis as normal |

Default: `Z`

_Visible when Write Vtx Normal = true_

</details>

<details>

<summary><strong>Include Vtx In OOB</strong> <code>bool</code></summary>

Include the vertex position itself in the oriented bounding box calculation.

Default: `false`

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| **Vtx**   | Points | Vertices with computed properties |
| **Edges** | Points | Unchanged edge data               |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/PCGExWriteVtxProperties.h)
