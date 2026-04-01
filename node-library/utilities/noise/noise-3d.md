---
description: Core base classes for the 3D noise system.
icon: arrow-down-from-arc
---

# Noise 3D

### Overview

This is the foundational infrastructure for PCGEx 3D noise generation, which provides procedural noise patterns for use in various operations. Noise generators can be stacked and blended together, with each contributing to a final combined value. The base configuration provides common settings for frequency, seeding, remapping, transformations, and blending modes that all noise types inherit.

### Base Configuration

All noise types inherit these common settings from `FPCGExNoise3DConfigBase`:

#### Common Noise Settings

<details>

<summary><strong>Weight Factor</strong> <code>double</code></summary>

The weight factor for this noise when combining multiple noise sources. Higher values increase this noise's contribution to the final blended result.

* **1.0** (default): Equal weight
* **2.0**: Double influence
* **0.5**: Half influence

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Mode</strong> <code>EPCGExNoiseBlendMode</code></summary>

Determines how this noise combines with other noises when stacked.

| Option       | Description                                           |
| ------------ | ----------------------------------------------------- |
| **Blend**    | Weighted average blend based on weight factors        |
| **Add**      | Additive blending (increases overall values)          |
| **Subtract** | Subtractive blending (decreases overall values)       |
| **Multiply** | Multiplicative blending (darkens, emphasizes valleys) |
| **Divide**   | Division blending                                     |
| **Min**      | Take minimum value (keeps lowest)                     |
| **Max**      | Take maximum value (keeps highest)                    |

Default: `Blend`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, inverts the noise output (1.0 - value), flipping peaks to valleys and vice versa.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remap Curve</strong> <code>FRuntimeFloatCurve</code></summary>

A curve used to remap the noise output values. The noise value (0-1) is used to sample this curve, allowing you to reshape the distribution.

* Use a linear curve (default) for no remapping
* Use an S-curve for contrast adjustment
* Use custom shapes to emphasize certain value ranges

Default: Linear curve from (0,0) to (1,1)

</details>

<details>

<summary><strong>Seed</strong> <code>int32</code></summary>

Random seed for the noise generator. Different seeds produce different but deterministic noise patterns. Use the same seed for reproducible results.

Default: `1337`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Transform</strong> <code>bool</code></summary>

When enabled, applies the specified Transform to the noise sampling coordinates before generating noise. This allows you to scale, rotate, or offset the noise pattern in 3D space.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform</strong> <code>FTransform</code></summary>

The transformation to apply to noise sampling coordinates. Useful for:

* **Scale**: Adjust noise detail (larger scale = larger features)
* **Rotation**: Rotate the noise pattern
* **Translation**: Offset the noise pattern

Default: `Identity` (no transformation)

📋 _Visible when Apply Transform is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Frequency</strong> <code>double</code></summary>

The base frequency of the noise, which controls the scale of features. Higher frequencies create smaller, more detailed patterns. Lower frequencies create larger, smoother patterns.

* **0.01** (default): Large, smooth features
* **0.1**: Medium-sized features
* **1.0**: Fine, detailed features

Default: `0.01`

Minimum: `0.000001`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Contrast</strong> <code>double</code></summary>

Adjusts the contrast of the noise output, controlling the difference between high and low values.

* **1.0** (default): No change
* **>1.0**: More contrast (values pushed toward 0 and 1)
* **<1.0**: Less contrast (values compressed toward 0.5)

Default: `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Contrast Curve</strong> <code>EPCGExContrastCurve</code></summary>

The curve function used to apply contrast adjustment.

| Option          | Description                               |
| --------------- | ----------------------------------------- |
| **Power**       | Power function (smooth, gradual)          |
| **Exponential** | Exponential function (sharp transitions)  |
| **Logarithmic** | Logarithmic function (gentle transitions) |

Default: `Power`

📋 _Visible when Contrast ≠ 1.0_

⚡ PCG Overridable

</details>

#### Provider Settings

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Noise priority, which affects the order of blending and weighting when multiple noises are combined. Lower priority values are applied first, higher priority values are applied last.

Default: `0`

⚡ PCG Overridable

</details>

### Factory Types

The noise system uses a factory pattern:

* **UPCGExNoise3DFactoryData**: Base factory class that creates noise operations
  * Stores configuration settings
  * Creates `FPCGExNoise3DOperation` instances
* **UPCGExNoise3DFactoryProviderSettings**: Base provider class that creates noise factories
  * Outputs to the `Noise3D` pin
  * Derived classes implement specific noise algorithms

### Noise Categories

Noise generators available in PCGEx:

* **Caustic**: Water caustic patterns
* **Curl**: Curl noise for fluid flow
* **FBM**: Fractional Brownian Motion (layered noise)
* **Flow**: Flow field noise
* **Gabor**: Gabor noise patterns
* **Marble**: Marble-like patterns
* **OpenSimplex2**: Open Simplex 2 noise
* **Perlin**: Classic Perlin noise
* **Simplex**: Simplex noise
* **Value**: Value noise
* **Voronoi**: Voronoi/cellular patterns
* **Wood**: Wood grain patterns

Each noise type extends these base classes and adds specific parameters while inheriting the common configuration settings above.

### Usage Pattern

```
Noise3D Provider Node
      ↓
Creates Factory (UPCGExNoise3DFactoryData)
      ↓
Creates Operation (FPCGExNoise3DOperation)
      ↓
Samples 3D positions → Noise values (0-1)
      ↓
Applies: Transform → Frequency → Contrast → Remap Curve
      ↓
Output: Processed noise value
```

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Core/PCGExNoise3DFactoryProvider.h)
