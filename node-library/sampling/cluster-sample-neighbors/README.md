---
description: Sample cluster vtx' neighbors values.
icon: circle-nodes
---

# Cluster : Sample Neighbors

### Overview

This node samples attribute values, properties, and other data from neighboring vertices in a cluster. It uses connected sampler sub-nodes to define what data to gather and how to blend it. Each vertex receives blended values from its graph neighbors, enabling operations like smoothing, averaging, or gathering neighborhood statistics.

### How It Works

1. **Sampler Collection**: Gathers all connected sampler sub-nodes from the Samplers input
2. **Neighbor Traversal**: For each vertex, identifies connected neighbors based on cluster edges
3. **Sampling Execution**: Each sampler processes the neighbors according to its configuration
4. **Result Writing**: Sampled and blended values are written to vertex attributes

**Usage Notes**

* **Sampler Sub-nodes Required**: Connect one or more sampler sub-nodes to the Samplers pin to define what to sample.
* **Multiple Samplers**: You can connect multiple samplers to gather different data types or use different blend modes.
* **Traversal Depth**: Samplers can be configured to look beyond immediate neighbors using the MaxDepth setting.

### Behavior

```
Cluster with Values         After Sampling
                            (Average blend)

    [5]                         [5]
     │                           │
[3]──•──[7]         →      [5]──•──[5]
     │                           │
    [5]                         [5]

Center vertex samples
neighbors and blends values.
```

### Inputs

| Pin          | Type              | Description                               |
| ------------ | ----------------- | ----------------------------------------- |
| **Vtx**      | Points            | Cluster vertex points                     |
| **Edges**    | Points            | Cluster edge data                         |
| **Samplers** | Sampler Factories | Sampler sub-nodes defining what to sample |

### Sampler Types

Connect these sub-nodes to the Samplers pin:

| Sampler                      | Description                                          |
| ---------------------------- | ---------------------------------------------------- |
| **Sampler : Vtx Blend**      | Use blend operations for complex attribute blending  |
| **Sampler : Test Neighbors** | Test neighbors against filters and output statistics |

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| **Vtx**   | Points | Vertices with sampled neighbor data |
| **Edges** | Points | Unchanged edge data                 |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/PCGExSampleNeighbors.h)
