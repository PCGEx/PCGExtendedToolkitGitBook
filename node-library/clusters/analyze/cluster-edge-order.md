---
description: Fix an order for edge start & end endpoints.
icon: share-nodes
---

# Cluster : Edge Order

### Overview

This node normalizes the direction of edges in a cluster by ensuring consistent start and end endpoint ordering. Edges in a graph can be stored with arbitrary endpoint order, and this node allows you to enforce a specific direction based on various criteria like attribute values or sorting rules.

### How It Works

1. **Direction Analysis**: Evaluates each edge according to the selected direction method
2. **Endpoint Swapping**: Swaps start/end indices where needed to match the desired direction
3. **Edge Update**: Writes the corrected endpoint ordering to edge data

**Usage Notes**

* **Preprocessing Step**: Use this node before operations that depend on consistent edge direction.
* **Attribute-Based**: Direction can be determined by comparing endpoint attribute values.
* **Sorting Rules**: Connect sorting nodes for complex multi-criteria direction rules.

### Inputs

| Pin       | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| **Vtx**   | Points | Cluster vertex points        |
| **Edges** | Points | Cluster edge data to reorder |

### Settings

#### Direction Settings

<details>

<summary><strong>Direction Settings</strong> <code>FPCGExEdgeDirectionSettings</code></summary>

Controls how edge direction is determined.

//→ See TODO FPCGExEdgeDirectionSettings

</details>

<details>

<summary><strong>Dir Source Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to compare for determining edge direction.

_Visible when Direction Method = EndpointsAttribute_

</details>

<details>

<summary><strong>Direction Choice</strong> <code>EPCGExEdgeDirectionChoice</code></summary>

When using attribute-based direction, whether edges should flow from smallest-to-greatest or greatest-to-smallest values.

| Option                 | Description                                |
| ---------------------- | ------------------------------------------ |
| **SmallestToGreatest** | Start endpoint has smaller attribute value |
| **GreatestToSmallest** | Start endpoint has larger attribute value  |

Default: `SmallestToGreatest`

_Visible when Direction Method = EndpointsAttribute_

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| **Vtx**   | Points | Unchanged vertex data                   |
| **Edges** | Points | Edges with normalized endpoint ordering |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExEdgeOrder.h)
