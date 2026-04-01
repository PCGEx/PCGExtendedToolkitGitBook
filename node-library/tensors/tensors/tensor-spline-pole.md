---
description: A tensor that represents a vector/flow field toward or away from a spline.
icon: circle-dashed
---

# Tensor : Spline Pole

### Overview

The Spline Pole Tensor creates a directional field where directions point toward or away from spline data. Unlike Spline Flow which provides directions along the spline, Spline Pole creates radial attraction or repulsion fields around the spline, where sample points are directed toward (or away from) the nearest point on the spline.

### How It Works

1. **Spline Input**: Receives spline data directly from the input pin.
2. **Closest Point Query**: For each sample position, finds the closest point on any input spline.
3. **Direction Calculation**: Calculates the direction vector from the sample point toward (or away from) the closest spline point.
4. **Falloff Application**: Applies distance-based falloff using the configured radius.

**Usage Notes**

* **Native Splines**: Works with actual spline components/data for precise geometry.
* **Attraction/Repulsion**: Creates pull toward or push away from splines depending on tensor weight sign and mutations.
* **Radius**: Defines the influence distance from the spline. Points beyond this radius receive no directional influence.

### Behavior

**Spline Pole Tensor (attraction):**

```
Input spline:  ╭──────────╮
               │          │
               ╰──────────╯

Tensor field:
  ↘ ↘ ↓ ↓ ↓ ↓ ↓ ↙ ↙
  → → ↘ ↓ ↓ ↓ ↙ ← ←
  → → → ↘ ↓ ↙ ← ← ←
        → ↓ ←
  → → → ↗ ↑ ↖ ← ← ←
  → → ↗ ↑ ↑ ↑ ↖ ← ←
  ↗ ↗ ↑ ↑ ↑ ↑ ↑ ↖ ↖

Directions point toward the nearest
point on the spline.
```

### Inputs

| Pin         | Type        | Description                                |
| ----------- | ----------- | ------------------------------------------ |
| **Splines** | Spline Data | Spline components defining the pole shapes |

### Settings

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base influence radius of the spline. Points beyond this distance receive no directional influence. The effective radius may be scaled by spline control point scale.

Default: `100`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits from the tensor spline factory provider base.

→ See Tensor Factory for common tensor options including Weight, Potency, and Falloff Curves.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorSplinePole.h)
