---
description: A tensor constant that uses the seed transform.
icon: circle-dashed
---

# Tensor : Inertia (Constant)

### Overview

The Inertia Constant Tensor provides a direction based on the sampling point's own transform with an optional rotational offset. Unlike the point-based Inertia tensor, this version doesn't require input points — it derives direction purely from the probe's transform at sample time. The rotational offset allows biasing the inertia direction relative to the probe's actual orientation.

### How It Works

{% hint style="info" %}
Unlike a Constant tensor, the direction of this tensors is fetched from the point sampling the field.
{% endhint %}

1. **Transform Read**: Reads the probe's current transform at sample time.
2. **Axis Extraction**: Extracts the specified axis from the transform rotation.
3. **Offset Application**: Applies the configured rotational offset to the direction.
4. **Direction Output**: Returns the offset-adjusted axis as the tensor direction.

**Usage Notes**

* **No Input Points**: This tensor doesn't use input points — it operates purely on the probe transform, making it simpler for basic inertia needs.
* **Rotational Offset**: The offset rotator allows constant angular deviation from the probe's axis, useful for creating spiral or curved extrusion patterns.
* **Set Once Mode**: When enabled, captures the direction at the start and keeps it constant, ignoring subsequent probe rotation changes.

### Behavior

**Inertia Constant with Offset:**

```
Probe facing → with Offset (0, 45, 0):

  Normal inertia: →
  With offset:    ↗ (rotated 45° around up axis)

The offset creates a consistent angular deviation
from the probe's natural direction.
```

### Settings

<details>

<summary><strong>Axis</strong> <code>EPCGExAxis</code></summary>

Which axis of the probe transform to use as the base inertia direction.

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

<summary><strong>Offset</strong> <code>FRotator</code></summary>

Rotational offset applied to the inertia direction. Use this to bias the direction relative to the probe's actual orientation.

Default: `(0, 0, 0)` (no offset)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tensor Weight</strong> <code>double</code></summary>

Base weight multiplier for this tensor. Controls how much this tensor contributes relative to others when combined.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Potency</strong> <code>double</code></summary>

Base potency multiplier for this tensor. Scales the magnitude of the tensor's influence.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Set Inertia Once</strong> <code>bool</code></summary>

When enabled, captures the inertia direction from the original seed point transform and uses it as a constant, rather than updating with the probe's current transform.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits from the tensor factory provider base.

→ See Tensor Factory for common tensor options.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorInertiaConstant.h)
