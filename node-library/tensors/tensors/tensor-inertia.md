---
description: A tensor constant that uses the seed transform.
icon: circle-dashed
---

# Tensor : Inertia

### Overview

The Inertia Tensor provides a direction based on the sampling point's own transform rather than external sources. When sampling, it returns a direction derived from a specified axis of the probe's transform, effectively representing the point's current "momentum" or facing direction. This allows extrusions to maintain their current heading when no other tensors have influence.

### How It Works

{% hint style="info" %}
Unlike a Constant tensor, the direction of this tensors is fetched from the point sampling the field.
{% endhint %}

1. **Transform Read**: Reads the probe's current transform at sample time.
2. **Axis Extraction**: Extracts the specified axis (Forward, Up, Right, etc.) from the transform.
3. **Direction Output**: Returns this axis as the tensor direction, representing inertia.
4. **Optional Lock**: Can optionally lock the inertia to the original point transform instead of updating with the probe.

**Usage Notes**

* **Momentum Preservation**: Use this tensor to maintain the current direction of travel when other tensors fade out.
* **Set Once Mode**: When enabled, captures the inertia direction from the original seed point and keeps it constant throughout extrusion, rather than updating as the probe moves.
* **Combination**: Often combined with other tensors — inertia provides a baseline direction that other tensors modify.

### Behavior

```
Inertia Tensor (using probe transform):

Probe at A facing →:    Returns →
Probe at B facing ↗:    Returns ↗
Probe at C facing ↑:    Returns ↑

Direction follows the probe's own orientation,
not external field sources.

With "Set Inertia Once" enabled:
All samples return the original seed direction,
regardless of how the probe rotates during extrusion.
```

### Inputs

| Pin    | Type       | Description                                                          |
| ------ | ---------- | -------------------------------------------------------------------- |
| **In** | Point Data | Points whose transforms define inertia reference (for Set Once mode) |

### Settings

<details>

<summary><strong>Axis</strong> <code>EPCGExAxis</code></summary>

Which axis of the point transform represents the inertia direction.

| Option       | Description                               |
| ------------ | ----------------------------------------- |
| **Forward**  | Local X axis (typical movement direction) |
| **Backward** | Negative local X axis                     |
| **Right**    | Local Y axis                              |
| **Left**     | Negative local Y axis                     |
| **Up**       | Local Z axis                              |
| **Down**     | Negative local Z axis                     |

Default: `Forward`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Set Inertia Once</strong> <code>bool</code></summary>

When enabled, captures the inertia direction from the original point transform and uses it as a constant throughout sampling. When disabled, the inertia direction updates based on the current probe transform at each sample.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits from the tensor point factory provider base, which includes weight, potency, and falloff curve settings.

→ See Tensor Factory for common tensor options including Weight, Potency, and Falloff Curves.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorInertia.h)
