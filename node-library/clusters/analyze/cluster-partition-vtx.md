---
description: Split Vtx into per-cluster groups.
icon: share-nodes
---

# Cluster : Partition Vtx

### Overview

This node separates a shared vertex collection into individual collections per cluster. When multiple edge collections reference the same vertex pool, this node creates separate vertex collections where each contains only the vertices used by its corresponding edge collection.

### How It Works

1. **Cluster Analysis**: Identifies which vertices belong to which cluster based on edge references
2. **Vertex Extraction**: Copies vertices into separate collections per cluster
3. **Index Remapping**: Updates edge endpoint indices to reference the new partitioned collections
4. **Output Generation**: Produces one vertex collection per edge collection

**Usage Notes**

* **Inverse of Merge Vtx**: This operation reverses what Merge Vtx does.
* **Per-Cluster Processing**: Enables independent processing of individual clusters.
* **Index Updates**: Edge indices are automatically remapped to the new vertex collections.

### Inputs

| Pin       | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| **Vtx**   | Points | Shared vertex collection                                  |
| **Edges** | Points | Multiple edge collections referencing the shared vertices |

### Settings

This node has no configurable settings. It simply partitions vertices based on cluster membership.

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| **Vtx**   | Points | Partitioned vertex collections (one per cluster) |
| **Edges** | Points | Edge collections with updated vertex references  |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExPartitionVertices.h)
