---
description: Curl noise - divergence-free for fluids and particles.
icon: circle-dashed
---

# Noise : Curl

### Overview

This noise generates a divergence-free vector field (∇·F = 0), meaning it has no sources or sinks where vectors converge or diverge. This creates natural swirling, flowing patterns perfect for simulating fluid motion, particle systems, smoke, clouds, and other phenomena that need continuous, circular flow without expansion or compression. The curl operation produces vectors that always flow around rather than toward or away from points.

### How It Works

1. **Potential Field Generation**: Creates a base noise field using octaves of simplex-like noise
2. **Derivative Calculation**: Computes partial derivatives of the potential field at each position using the Epsilon parameter
3. **Curl Operation**: Applies the curl operator (∇×F) to the potential field, which produces a divergence-free vector field
4. **Vector Output**: Returns 3D vectors that represent flow direction and magnitude at each position
5. **Scaling**: Multiplies the curl vectors by CurlScale for magnitude control
6. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Divergence-Free Property:

Traditional Noise (has divergence):
    ← ← ● → →  (vectors converge/diverge at points)
        ↓↓↓

Curl Noise (no divergence):
    ↻ → → ↻   (vectors flow around, never converge)
    ↑     ↓
    ↻ ← ← ↻
```

```
Octaves Effect:

1 Octave:  Smooth, large-scale swirls
3 Octaves: Medium detail with smaller turbulence
8 Octaves: Very turbulent, chaotic flow with fine details
```

```
Lacunarity & Persistence:

Low Lacunarity (1.5):   Gentle frequency increase per octave
High Lacunarity (4.0):  Sharp frequency jumps, more variation
Low Persistence (0.2):  Higher octaves contribute less (smooth)
High Persistence (0.8): Higher octaves contribute more (turbulent)
```

Good for: fluid flow, particle advection, smoke simulation, cloud motion, swirling effects

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine. Each octave adds finer detail at higher frequency.

* **1** (default): Single layer, smooth flow
* **3-4**: Good detail without excessive complexity
* **8**: Maximum detail, very turbulent

Each octave approximately doubles computation cost.

Default: `1`

Range: `1` to `8`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lacunarity</strong> <code>double</code></summary>

Frequency multiplier between octaves. Controls how quickly detail increases with each octave.

* **1.5**: Gentle frequency progression
* **2.0** (default): Standard doubling per octave
* **4.0**: Aggressive frequency increase, more contrast between scales

Default: `2.0`

Range: `1.0` to `4.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Persistence</strong> <code>double</code></summary>

Amplitude multiplier between octaves, controlling how much each successive octave contributes to the final result.

* **0.2**: Higher octaves barely visible (smooth)
* **0.5** (default): Balanced detail contribution
* **0.8**: Higher octaves dominant (turbulent, rough)

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Epsilon</strong> <code>double</code></summary>

Step size used for numerical derivative computation when calculating the curl. Smaller values give more precise derivatives but may be more sensitive to numerical errors.

* **0.0001**: Very precise (may have numerical issues)
* **0.001** (default): Good balance
* **0.1**: Coarse approximation (faster but less accurate)

Default: `0.001`

Range: `0.0001` to `0.1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Curl Scale</strong> <code>double</code></summary>

Multiplier for the magnitude of the curl vectors. Larger values create stronger, faster flow patterns.

* **0.1**: Very subtle flow
* **1.0** (default): Natural flow strength
* **10.0**: Very strong, intense flow

Default: `1.0`

Range: `0.1` to `10.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Note**: Curl noise primarily outputs vector fields. When used as a scalar value, it returns the magnitude of the curl vector.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseCurl.h)
