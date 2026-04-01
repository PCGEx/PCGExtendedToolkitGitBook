---
description: Heuristics based on tensors.
icon: circle-dashed
---

# Heuristics : Tensor

### Overview

The Tensor Heuristic scores pathfinding edges based on their alignment with a tensor field. Edges that follow the tensor field direction receive better scores, biasing pathfinding to flow along field lines. This enables tensor-guided pathfinding where paths naturally follow flow patterns, avoid certain regions, or prefer specific directions.

### How It Works

1. **Tensor Sampling**: At each edge, samples the tensor field at the starting node position.
2. **Direction Comparison**: Computes the dot product between the edge direction and the tensor direction.
3. **Score Generation**: Converts the dot product into a score — higher alignment yields lower cost (better path).
4. **Weight Application**: Applies the heuristic weight factor to balance against other heuristics.

**Usage Notes**

* **Absolute Mode**: When enabled, treats edges as favorable if they align with the tensor regardless of direction (both +1 and -1 dot products are good). When disabled, only edges matching the tensor direction are favored.
* **Combination**: Works well with Distance heuristics to balance "follow the flow" against "shortest path."

### Behavior

```
Tensor Field: → → → (pointing right)

Edge A→B (right):  Dot = 1.0   → Low cost (favorable)
Edge A→C (up):     Dot = 0.0   → Medium cost (neutral)
Edge A→D (left):   Dot = -1.0  → High cost (unfavorable) or Low if Absolute

Pathfinding will prefer edges aligned with the tensor field.
```

### Inputs

| Pin         | Type             | Description                                    |
| ----------- | ---------------- | ---------------------------------------------- |
| **Tensors** | Tensor Factories | Tensor field definitions for direction scoring |

### Settings

<details>

<summary><strong>Absolute</strong> <code>bool</code></summary>

When enabled, uses the absolute value of the dot product. This means edges aligned with the tensor in either direction (forward or backward) are scored equally. When disabled, only edges matching the tensor's positive direction are favored.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tensor Sampling Settings</strong> <code>FPCGExTensorHandlerDetails</code></summary>

Settings for how tensors are sampled at each node.

| Property             | Type     | Description                      |
| -------------------- | -------- | -------------------------------- |
| **Invert**           | `bool`   | Invert the tensor direction      |
| **Normalize**        | `bool`   | Normalize the tensor result      |
| **Size**             | `double` | Scale factor for tensor sampling |
| **Sampler Settings** | Struct   | Tensor sampler configuration     |

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits from the heuristics factory base.

→ See Heuristics Factory for common heuristic options including Weight Factor and Score Curve.

### Outputs

| Pin            | Type               | Description                                         |
| -------------- | ------------------ | --------------------------------------------------- |
| **Heuristics** | Heuristics Factory | Heuristic definition for use with pathfinding nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Heuristics/PCGExHeuristicTensor.h)
