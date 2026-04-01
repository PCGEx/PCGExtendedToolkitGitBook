---
description: Restores vtx/edge clusters from packed dataset.
icon: share-nodes
---

# Cluster : Unpack

### Overview

This node reverses the packing operation performed by Pack Clusters. It takes a single packed point collection and separates it back into distinct vertex and edge outputs, restoring the original cluster structure. This is useful when you need to perform cluster-specific operations on data that was previously packed for storage or transfer.

### How It Works

1. **Read Packed Data**: Reads the packed cluster data containing both vertex and edge information.
2. **Extract Metadata**: Parses the packing metadata to identify cluster boundaries and relationships.
3. **Separate Components**: Splits the packed points back into separate vertex and edge collections.
4. **Restore Structure**: Rebuilds the cluster topology with proper vertex-edge associations.

**Usage Notes**

* **Paired with Pack**: Use this node to restore clusters that were previously combined using Pack Clusters.
* **Flatten Option**: Enable flattening for faster processing when you don't need to preserve nested metadata, at the cost of additional memory during unpacking.

### Behavior

```
Packed Input:                   Unpacked Output:

[Packed Clusters]               Vtx:  [Cluster A Vertices]
  Contains:                           [Cluster B Vertices]
  - Cluster A (vtx + edges)
  - Cluster B (vtx + edges)     Edges: [Cluster A Edges]
                                       [Cluster B Edges]
```

### Inputs

| Pin                 | Type   | Description                   |
| ------------------- | ------ | ----------------------------- |
| **Packed Clusters** | Points | Packed cluster data to unpack |

### Settings

<details>

<summary><strong>Flatten</strong> <code>bool</code></summary>

Flatten unpacked metadata. This is a tradeoff between memory and speed — flattening uses more memory during unpacking but can improve processing speed for subsequent operations.

Default: `false`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type   | Description               |
| --------- | ------ | ------------------------- |
| **Vtx**   | Points | Restored cluster vertices |
| **Edges** | Points | Restored cluster edges    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExUnpackClusters.h)
