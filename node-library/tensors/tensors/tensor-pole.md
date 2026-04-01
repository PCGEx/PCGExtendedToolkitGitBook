---
description: A tensor that pulls and/or pushes.
icon: circle-dashed
---

# Tensor : Pole

### Overview

The Pole Tensor creates radial attraction or repulsion fields around input points. Each effector point acts as a pole that either pulls sample points toward it or pushes them away, depending on weight and mutation settings. This is one of the most fundamental tensor types for creating point-based directional fields.

### How It Works

1. **Effector Setup**: Builds an array of pole effectors from input points with their positions and influence settings.
2. **Distance Calculation**: For each sample, calculates the direction and distance to nearby pole effectors.
3. **Direction Determination**: Points the direction toward (attraction) or away from (repulsion) each pole.
4. **Falloff Application**: Applies weight and potency falloff based on distance to create smooth influence gradients.

**Usage Notes**

* **Attraction vs Repulsion**: The direction can be inverted using the Sampling Mutations settings to switch between pull and push behavior.
* **Multiple Poles**: When multiple pole effectors overlap, their influences are combined based on the blending mode.
* **Falloff Curves**: Use falloff curves to control whether poles have hard or soft edges of influence.

### Behavior

**Pole Tensor (attraction mode):**

```
       ↖ ↑ ↗
        ╲│╱
    ← ── ● ── →    ● = Pole effector
        ╱│╲
       ↙ ↓ ↘

Directions point toward the pole.
With inversion, directions point away.
```

### Inputs

| Pin    | Type       | Description                             |
| ------ | ---------- | --------------------------------------- |
| **In** | Point Data | Points defining pole effector positions |

### Settings

This node uses inherited settings from the tensor point factory base for controlling effector influence.

#### Inherited Settings

→ See Tensor Factory for common tensor options including Weight, Potency, Falloff Curves, and Sampling Mutations.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorPole.h)
