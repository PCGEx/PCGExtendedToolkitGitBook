---
description: Merge Vtx so all edges share the same vtx collection.
icon: share-nodes
---

# Cluster : Merge Vtx

### Overview

This node consolidates multiple vertex point collections into a single unified collection. When you have clusters with separate vertex collections, this node merges them together and updates all edge references accordingly, allowing edges from different clusters to share a common vertex pool.

### How It Works

1. **Vertex Consolidation**: Combines all input vertex collections into one
2. **Index Remapping**: Updates edge endpoint indices to reference the merged collection
3. **Attribute Merging**: Carries over vertex attributes according to filter settings
4. **Output Generation**: Produces a single Vtx collection with updated edge collections

**Usage Notes**

* **Prerequisite for Operations**: Some operations require all edges to reference the same vertex collection.
* **Index Adjustment**: Edge endpoint indices are automatically adjusted to the new merged positions.
* **Attribute Handling**: Configure Carry Over Settings to control which attributes are preserved.

### Inputs

| Pin       | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| **Vtx**   | Points | Multiple vertex point collections to merge      |
| **Edges** | Points | Edge collections referencing the input vertices |

### Settings

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are preserved during the merge.

//→ See TODO FPCGExCarryOverDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| **Vtx**   | Points | Single merged vertex collection                 |
| **Edges** | Points | Edge collections with updated vertex references |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExMergeVertices.h)
