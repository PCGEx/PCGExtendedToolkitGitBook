---
description: A tensor that uses 3D noises as direction, within effector bounds.
icon: circle-dashed
---

# Tensor : Noise (Bounded)

### Overview

The Bounded Noise Tensor generates directions from 3D noise functions, but limits the noise influence to regions defined by input points. Each input point acts as an effector with its own influence radius and falloff, creating localized pockets of noise-driven flow rather than a global noise field. This allows precise control over where noise affects the tensor field.

### How It Works

1. **Effector Setup**: Builds an effector array from input points, each with position and influence settings.
2. **Noise Initialization**: Initializes the noise generator(s) from the input pin.
3. **Bounded Sampling**: When sampled, checks proximity to effector points and only applies noise within their influence range.
4. **Falloff Application**: Applies weight and potency falloff based on distance to the nearest effector.

**Usage Notes**

* **Localized Noise**: Unlike the global Noise tensor, this version only produces noise directions near input points.
* **Effector Influence**: The falloff curves control how noise strength decreases with distance from each effector point.
* **Noise Input Required**: Connect a Noise Factory to define the noise characteristics.

### Behavior

**Bounded Noise Tensor:**

```
    Global space:  (no influence)
         │
    ┌────●────┐   Effector point with radius
    │ ↗ ↑ ↖  │
    │ → ● ←  │   Noise active within bounds
    │ ↘ ↓ ↙  │
    └─────────┘
         │
    Global space:  (no influence)

Noise only affects regions near effector points.
```

### Inputs

| Pin            | Type          | Description                                        |
| -------------- | ------------- | -------------------------------------------------- |
| **In**         | Point Data    | Points defining effector positions and bounds      |
| **Noise**      | Noise Factory | 3D noise generator defining the direction field    |
| **Noise Mask** | Noise Factory | Optional noise to mask the primary noise influence |

### Settings

<details>

<summary><strong>Normalize Noise Sampling</strong> <code>bool</code></summary>

When enabled, normalizes the sampled noise direction to unit length. This ensures consistent direction magnitude regardless of the raw noise amplitude.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits from the tensor point factory provider base, which includes weight, potency, and falloff curve settings that control effector influence.

→ See Tensor Factory for common tensor options including Weight, Potency, and Falloff Curves.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorNoiseBounded.h)
