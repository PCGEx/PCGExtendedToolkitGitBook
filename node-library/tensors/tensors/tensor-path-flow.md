---
description: A tensor that represents a vector/flow field along a path.
icon: circle-dashed
---

# Tensor : Path Flow

### Overview

The Path Flow Tensor creates a directional field that follows paths defined by input points. Each path is treated as a spline, and the tensor provides directions that follow along the path's tangent at any sample point. This creates flow fields that guide movement along curved routes defined by point sequences.

### How It Works

1. **Path Conversion**: Converts input point sequences into spline representations using the specified point type.
2. **Closest Point Query**: For each sample position, finds the closest point on any path spline.
3. **Tangent Extraction**: Extracts the spline tangent direction at the closest point.
4. **Falloff Application**: Applies distance-based falloff from the path using the configured radius.

**Usage Notes**

* **Path-Based Input**: Input points are treated as ordered paths — each collection becomes a separate spline.
* **Point Type**: Controls spline interpolation (Linear, Curve, etc.). Linear with smoothing provides a balance between performance and smooth flow.
* **Radius**: Defines the influence distance from the path. Points beyond this radius receive no flow direction.

### Behavior

**Path Flow Tensor:**

```
Input path:  ●───●───●───●
                 ╲
Tensor field:     ╲
  → → → ↘ ↘ ↘ → → →
  → → → → → → → → →
  → → → ↗ ↗ ↗ → → →
                 ╱
                ╱
             ●───●

Directions follow the path tangent,
with falloff based on distance from path.
```

### Inputs

| Pin    | Type       | Description                                                |
| ------ | ---------- | ---------------------------------------------------------- |
| **In** | Point Data | Points defining paths (each collection is a separate path) |

### Settings

<details>

<summary><strong>Point Type</strong> <code>EPCGExSplinePointTypeRedux</code></summary>

Which interpolation type to use for path points. Shared across all points in the path.

| Option       | Description                      |
| ------------ | -------------------------------- |
| **Linear**   | Straight segments between points |
| **Curve**    | Smooth curved interpolation      |
| **Constant** | Step-wise constant direction     |

Default: `Linear`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smooth Linear</strong> <code>bool</code></summary>

When enabled, smooths the direction transitions at linear segment joints for more natural flow.

Default: `true`

📋 _Visible when Point Type = Linear_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base influence radius of the path. Points beyond this distance receive no flow direction. The effective radius may be scaled by control point scale.

Default: `100`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spline Direction</strong> <code>EPCGExAxis</code></summary>

Which axis of the spline's local transform represents the flow direction.

| Option         | Description                           |
| -------------- | ------------------------------------- |
| **Forward**    | Flow along path tangent               |
| **Backward**   | Flow against path tangent             |
| **Right/Left** | Flow perpendicular to path            |
| **Up/Down**    | Flow perpendicular to path (vertical) |

Default: `Forward`

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorPathFlow.h)
