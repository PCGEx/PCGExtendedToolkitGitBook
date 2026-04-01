---
description: A tensor that represents a vector/flow field along a spline.
icon: circle-dashed
---

# Tensor : Spline Flow

### Overview

The Spline Flow Tensor creates a directional field that follows spline data. Unlike Path Flow which converts points to splines, this tensor works directly with spline input, providing precise control over flow direction using native spline tangents and transforms. The tensor provides directions that follow along the spline at any sample point within the influence radius.

### How It Works

1. **Spline Input**: Receives spline data directly from the input pin.
2. **Closest Point Query**: For each sample position, finds the closest point on any input spline.
3. **Tangent Extraction**: Extracts the spline tangent direction at the closest point using the specified axis.
4. **Falloff Application**: Applies distance-based falloff from the spline using the configured radius.

**Usage Notes**

* **Native Splines**: Works with actual spline components/data, preserving full spline fidelity including custom tangents.
* **Radius**: Defines the influence distance from the spline. Points beyond this radius receive no flow direction.
* **Direction Axis**: Choose which spline transform axis represents the flow direction.

### Behavior

**Spline Flow Tensor:**

```
Input spline:  ╭──────────╮
               │          │
               ╰──────────╯

Tensor field:
  → → ↗ ↑ ↖ ← ← ←
  → → → ↗ ↑ ↖ ← ←
  → → → → ↗ ↖ ← ←
        ↘ → ↗
        → → →

Directions follow the spline tangent,
with falloff based on distance from spline.
```

### Inputs

| Pin         | Type        | Description                               |
| ----------- | ----------- | ----------------------------------------- |
| **Splines** | Spline Data | Spline components defining the flow paths |

### Settings

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base influence radius of the spline. Points beyond this distance receive no flow direction. The effective radius may be scaled by spline control point scale.

Default: `100`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spline Direction</strong> <code>EPCGExAxis</code></summary>

Which axis of the spline's local transform represents the flow direction.

| Option         | Description                             |
| -------------- | --------------------------------------- |
| **Forward**    | Flow along spline tangent               |
| **Backward**   | Flow against spline tangent             |
| **Right/Left** | Flow perpendicular to spline            |
| **Up/Down**    | Flow perpendicular to spline (vertical) |

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorSplineFlow.h)
