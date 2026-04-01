---
icon: circle-dashed
---

# Tensor : Noise

### Overview

The Noise Tensor generates directions from 3D noise functions, creating organic, spatially-varying flow fields. The noise is sampled at each probe position to produce a direction vector, enabling natural-looking variations that change smoothly across space. Different noise types (Perlin, Simplex, etc.) produce different flow characteristics.

### How It Works

1. **Noise Setup**: Initializes the configured noise generator(s) from the input pin.
2. **Position Sampling**: At each probe position, samples the 3D noise function.
3. **Direction Derivation**: Converts the noise sample into a direction vector.
4. **Normalization**: Optionally normalizes the result to unit length for consistent direction regardless of noise amplitude.

**Usage Notes**

* **Noise Input Required**: Connect a Noise Factory to the input pin to define the noise characteristics (type, frequency, octaves, etc.).
* **Mask Support**: An optional second noise input can mask the primary noise, creating regions of varying influence.
* **Spatial Coherence**: Noise-based directions change smoothly across space, creating organic flow patterns without sharp discontinuities.

### Behavior

```
Noise Tensor Field (conceptual):

  ↗ → ↘ ↓ ↙
  ↑ ↗ → ↘ ↓
  ↗ ↑ ↗ → ↘
  → ↗ ↑ ↗ →
  ↘ → ↗ ↑ ↗

Directions vary smoothly based on noise
sampled at each position.
```

### Inputs

| Pin            | Type          | Description                                        |
| -------------- | ------------- | -------------------------------------------------- |
| **Noise**      | Noise Factory | 3D noise generator defining the direction field    |
| **Noise Mask** | Noise Factory | Optional noise to mask the primary noise influence |

### Settings

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

<summary><strong>Normalize Noise Sampling</strong> <code>bool</code></summary>

When enabled, normalizes the sampled noise direction to unit length. This ensures consistent direction magnitude regardless of the raw noise amplitude.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sampling Mutations</strong> <code>FPCGExTensorSamplingMutationsDetails</code></summary>

Tensor mutations settings that modify the sampled result.

| Property                     | Type      | Description                              |
| ---------------------------- | --------- | ---------------------------------------- |
| **Invert**                   | `bool`    | Flip the tensor direction                |
| **Bidirectional**            | `bool`    | Mirror influence based on reference axis |
| **Scale Direction And Size** | `bool`    | Apply custom scaling to direction        |
| **Scale**                    | `FVector` | Scale multiplier per axis                |

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorNoise.h)
