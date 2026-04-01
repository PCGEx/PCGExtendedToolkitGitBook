---
description: Flow noise - time-coherent animated patterns.
icon: circle-dashed
---

# Noise : Flow

### Overview

This noise creates time-coherent animated patterns where the noise evolves smoothly over time rather than jumping between unrelated values. It works by rotating the gradient vectors at each grid point rather than changing them randomly, producing continuous, flowing motion. This makes it ideal for animated effects like clouds drifting, water flowing, smoke rising, or fire flickering without the temporal artifacts that appear when using standard noise with a changing time parameter.

### How It Works

1. **Base Grid Setup**: Establishes a standard noise grid with gradient vectors at each lattice point
2. **Gradient Rotation**: Instead of randomizing gradients, rotates each gradient vector in the XY plane based on the Time parameter
3. **Per-Cell Rotation Rate**: Each grid point has a unique rotation speed (derived from its hash) multiplied by RotationSpeed, creating varied motion
4. **Octave Layering**: Optionally combines multiple rotated noise layers at different frequencies for more complex motion
5. **Interpolation**: Smoothly interpolates between rotated gradients as in standard gradient noise
6. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Traditional Time-Varying Noise (flickering):

Time 0: ~~~~~~~~~~
Time 1: :::::::::  (completely different pattern)
Time 2: ≈≈≈≈≈≈≈≈≈  (jumps randomly)
```

```
Flow Noise (smooth evolution):

Time 0: ~~~~~~~~~~
Time 1: ~≈≈≈≈≈≈≈~~  (gradual rotation)
Time 2: ≈≈≈≈≈≈≈≈≈≈  (continuous transformation)
```

```
Rotation Speed Effect:

Low (0.5):  Slow, gentle drifting
Med (1.0):  Natural flowing motion
High (10.0): Fast, turbulent swirling
```

```
Octaves for Complexity:

1 Octave:  Large-scale smooth flow
4 Octaves: Complex turbulent motion
16 Octaves: Highly detailed chaotic flow
```

Good for: animated clouds, flowing water, smoke, fire, wind effects, time-varying patterns

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine. Each octave adds finer detail at progressively higher frequency, creating more complex flow patterns.

* **1** (default): Simple, large-scale flow
* **4**: Good complexity for natural effects
* **16**: Maximum detail (expensive)

Each octave approximately doubles computation cost.

Default: `1`

Range: `1` to `16`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lacunarity</strong> <code>double</code></summary>

Frequency multiplier between successive octaves. Controls the scale variation between layers.

* **1.5**: Gentle frequency progression
* **2.0** (default): Standard doubling per octave
* **4.0**: Aggressive frequency increase, more turbulent

Default: `2.0`

Range: `1.0` to `4.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Persistence</strong> <code>double</code></summary>

Amplitude multiplier between successive octaves, controlling how much each layer contributes.

* **0.2**: Smooth, large features dominate
* **0.5** (default): Balanced detail contribution
* **0.9**: Rough, fine details prominent

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Time</strong> <code>double</code></summary>

Time parameter that drives the animation. As this value increases, gradient vectors rotate, creating the flowing motion effect.

* **0.0** (default): Initial state
* **Increasing values**: Continuous animated flow

Typically driven by actual time (seconds, frames) or a custom animation parameter. The pattern evolves smoothly and continuously as Time advances.

Default: `0.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation Speed</strong> <code>double</code></summary>

Multiplier controlling how quickly the gradients rotate. Higher values create faster-moving, more dynamic patterns.

* **0.1**: Very slow drift
* **1.0** (default): Natural flow speed
* **10.0**: Very fast, energetic motion

Each grid point has a unique rotation rate (0.5 to 1.5 times this value) to create varied, organic motion rather than uniform rotation.

Default: `1.0`

Range: `0.0` to `10.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: For smooth, non-flickering animation loops, set Time to a value that cycles (e.g., `Time % LoopDuration`) to create seamless repeating patterns.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseFlow.h)
