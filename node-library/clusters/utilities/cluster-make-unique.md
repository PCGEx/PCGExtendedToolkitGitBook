---
description: >-
  Outputs a new, unique data pointer for the input clusters; to avoid overlap
  and unexpected behaviors.
icon: share-nodes
---

# Cluster : Make Unique

### Overview

This utility node creates fresh, independent copies of cluster data with new unique identifiers. When PCG data is passed through multiple graph branches, they may share the same underlying data pointer, which can cause unexpected modifications to propagate between branches. This node ensures complete data independence.

### How It Works

1. **Data Duplication**: Creates new point collections with unique identifiers
2. **Metadata Refresh**: Assigns new cluster metadata to break any shared references
3. **Pass-through**: All vertex and edge data is preserved in the new collections

**Usage Notes**

* **Graph Branches**: Use when the same cluster data feeds into multiple processing branches.
* **Data Isolation**: Prevents modifications in one branch from affecting another.
* **No Transformation**: Data content is preserved exactly; only the data pointer changes.

{% hint style="info" %}
### When to Use This Node

Use Make Unique when you:

* Fork cluster data into parallel processing paths
* Need to modify clusters independently in different branches
* Experience unexpected data sharing between graph sections
{% endhint %}

### Inputs

| Pin       | Type   | Description           |
| --------- | ------ | --------------------- |
| **Vtx**   | Points | Cluster vertex points |
| **Edges** | Points | Cluster edge data     |

### Settings

This node has no configurable settings. It simply creates unique copies of the input data.

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                |
| --------- | ------ | -------------------------- |
| **Vtx**   | Points | Unique copy of vertex data |
| **Edges** | Points | Unique copy of edge data   |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExMakeClustersUnique.h)
