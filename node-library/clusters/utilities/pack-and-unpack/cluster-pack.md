---
description: >-
  Pack each cluster into a single point data object containing both vtx and
  edges.
icon: circle
---

# Cluster : Pack

### Overview

This node consolidates cluster data by combining vertices and edges into unified point collections. Each cluster becomes a single packed data object, simplifying data flow and enabling storage or transfer of complete cluster structures. This is the inverse of the Unpack operation.

### How It Works

1. **Data Collection**: Gathers vertex and edge data for each cluster
2. **Point Merging**: Combines both into a single point collection with metadata
3. **Attribute Preservation**: Carries over selected attributes according to filter settings
4. **Output Generation**: Produces packed cluster objects

**Usage Notes**

* **Serialization**: Packed clusters are easier to store or pass through single-pin connections.
* **Unpack Required**: Use the Unpack node to restore separate Vtx/Edges before cluster operations.
* **Cluster Integrity**: Each packed object contains all data needed to reconstruct the cluster.

### Inputs

| Pin       | Type   | Description           |
| --------- | ------ | --------------------- |
| **Vtx**   | Points | Cluster vertex points |
| **Edges** | Points | Cluster edge data     |

### Settings

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are preserved during packing.

//→ See TODO FPCGExCarryOverDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin                 | Type   | Description                 |
| ------------------- | ------ | --------------------------- |
| **Packed Clusters** | Points | Packed cluster data objects |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExPackClusters.h)
