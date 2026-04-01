---
description: A tensor that represents a vector/flow field toward or away from a path.
icon: circle-dashed
---

# Tensor : Path Pole

### Overview

The Path Pole Tensor creates a directional field where directions point toward or away from paths defined by input points. Unlike Path Flow which provides directions along the path, Path Pole creates radial attraction or repulsion fields around the path, where sample points are directed toward (or away from) the nearest point on the path.

### How It Works

1. **Path Conversion**: Converts input point sequences into spline representations.
2. **Closest Point Query**: For each sample position, finds the closest point on any path spline.
3. **Direction Calculation**: Calculates the direction vector from the sample point toward (or away from) the closest path point.
4. **Falloff Application**: Applies distance-based falloff using the configured radius.

**Usage Notes**

* **Attraction/Repulsion**: Creates pull toward or push away from paths depending on tensor weight sign and mutations.
* **Path-Based Input**: Input points are treated as ordered paths — each collection becomes a separate spline.
* **Radius**: Defines the influence distance from the path. Points beyond this radius receive no directional influence.

### Behavior

**Path Pole Tensor (attraction):**

```
Input path:  ●───●───●───●
                 │
Tensor field:    │
  ↘ ↘ ↘ ↓ ↓ ↓ ↙ ↙ ↙
  → → → → → → ← ← ←
  ↗ ↗ ↗ ↑ ↑ ↑ ↖ ↖ ↖
                 │
                 │
             ●───●

Directions point toward the nearest
point on the path.
```

### Inputs

| Pin    | Type       | Description                                                |
| ------ | ---------- | ---------------------------------------------------------- |
| **In** | Point Data | Points defining paths (each collection is a separate path) |

### Settings

<details>

<summary><strong>Point Type</strong> <code>EPCGExSplinePointTypeRedux</code></summary>

Which interpolation type to use for path points.

| Option       | Description                      |
| ------------ | -------------------------------- |
| **Linear**   | Straight segments between points |
| **Curve**    | Smooth curved interpolation      |
| **Constant** | Step-wise constant               |

Default: `Linear`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smooth Linear</strong> <code>bool</code></summary>

When enabled, smooths the direction transitions at linear segment joints.

Default: `true`

📋 _Visible when Point Type = Linear_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base influence radius of the path. Points beyond this distance receive no directional influence. The effective radius may be scaled by control point scale.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorPathPole.h)
