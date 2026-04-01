---
description: >-
  Create a vertex attribute sampler that uses blend operations to blend values
  from neighbors.
icon: circle-dashed
---

# Sampler : Vtx Blend

### Overview

This sub-node creates a sampler that uses connected Blend Operation sub-nodes to control how attributes are sampled and combined from neighboring vertices. Unlike the simpler Vtx Attributes sampler, this version allows full control over blending through modular blend operation factories, enabling complex multi-attribute sampling with different blend modes per attribute.

### How It Works

1. **Connect Blend Operations**: Attach Blend Op sub-nodes defining which attributes to blend and how
2. **Configure Sampling**: Set base sampling parameters (inherited from provider)
3. **Connect to Sampler**: Provide this factory to a Sample Neighbors node
4. **Execute Sampling**: Each blend operation processes its target attribute during neighbor traversal
5. **Finalize Results**: Blended values are written back to vertex attributes

**Usage Notes**

* **Modular Blending**: Each Blend Op sub-node can target different attributes with independent blend modes.
* **Input-Driven**: Blend operations are connected via input pins rather than configured inline, allowing reuse and complex setups.

### Behavior

```
Blend Operation Sampling:

Blend Op Inputs:
  ├─ Blend Op (Average): "Health"
  ├─ Blend Op (Max): "Damage"
  └─ Blend Op (Sum): "Score"

Vertex A with neighbors B, C:
  B: Health=80, Damage=20, Score=100
  C: Health=60, Damage=30, Score=150

Results on A:
  Health = Average(80, 60) = 70
  Damage = Max(20, 30) = 30
  Score = Sum(100, 150) = 250

Each attribute uses its own blend operation's rules.
```

### Inputs

| Pin           | Type               | Description                                           |
| ------------- | ------------------ | ----------------------------------------------------- |
| **Blend Ops** | Blend Op Factories | Blend operation sub-nodes defining attribute blending |

### Outputs

| Pin     | Type    | Description                                            |
| ------- | ------- | ------------------------------------------------------ |
| **Out** | Sampler | Neighbor sampler factory for use with Sample Neighbors |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/NeighborSamplers/PCGExNeighborSampleBlend.h)
