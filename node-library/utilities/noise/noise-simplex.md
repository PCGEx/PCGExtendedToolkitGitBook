---
description: Simplex gradient noise - efficient, high quality.
icon: circle-dashed
---

# Noise : Simplex

### Overview

This noise implements Simplex noise, Ken Perlin's improved noise algorithm designed to be faster and more computationally efficient than his original 1985 Perlin noise. Simplex noise operates on a simpler underlying geometry (simplexes rather than hypercubes), reducing the number of gradient evaluations needed and providing better performance, especially in higher dimensions. While it offers excellent quality and speed, it has patent restrictions that were only recently expired (expired in 2022), which led to the creation of patent-free alternatives like OpenSimplex2.

### How It Works

1. **Coordinate Skewing**: Applies a skew transformation to map input coordinates onto a simplex grid
2. **Simplex Identification**: Determines which simplex cell (triangle in 2D, tetrahedron in 3D) contains the sample point
3. **Corner Selection**: Identifies the simplex corners (4 in 3D) that contribute to the final value
4. **Gradient Evaluation**: For each corner, computes a gradient contribution using:
   * Distance-based attenuation with radial falloff (0.6 - r²)⁴
   * Dot product between gradient and position offset
5. **Contribution Sum**: Accumulates contributions from all simplex corners
6. **Octave Layering**: Optionally combines multiple octaves at different frequencies for fractal detail
7. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Characteristics:

- Fast computation (fewer gradient evaluations than Perlin)
- Smooth, continuous gradients
- Good visual quality with minimal directional artifacts
- Efficient scaling to higher dimensions
```

```
Octaves Effect:

1 Octave:  ~~~~~~~~~~~~  (smooth, large features)
4 Octaves: ≈≈≈≈≈≈≈≈≈≈≈≈  (good detail, natural)
16 Octaves: ::::::::::::  (very detailed, complex)
```

```
Comparison to Other Gradient Noises:

Perlin:        Original algorithm, can show grid artifacts
Simplex:       Faster, better quality, patent concerns (expired 2022)
OpenSimplex2:  Patent-free alternative, similar quality to Simplex
```

```
Performance:

Simplex is generally faster than Perlin, especially with multiple
octaves, due to fewer gradient lookups per sample.
```

Good for: general-purpose noise, terrain, textures, procedural effects where performance matters

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine for fractal detail. Each octave adds finer detail at progressively higher frequency.

* **1** (default): Single layer, smooth base noise
* **4**: Good balance of detail and performance
* **8**: High detail
* **16**: Maximum detail (expensive)

Each octave approximately doubles computation cost.

Default: `1`

Range: `1` to `16`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lacunarity</strong> <code>double</code></summary>

Frequency multiplier between successive octaves. Controls how quickly detail scales up with each additional layer.

* **1.5**: Gentle frequency progression
* **2.0** (default): Standard doubling per octave (classic fBm)
* **4.0**: Aggressive frequency increase

Default: `2.0`

Range: `1.0` to `4.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Persistence</strong> <code>double</code></summary>

Amplitude multiplier between successive octaves, controlling the contribution of each layer to the final result.

* **0.2**: Low persistence, smooth (large features dominate)
* **0.5** (default): Balanced, natural fractal distribution
* **0.9**: High persistence, rough (small features prominent)

Also known as "gain" in some noise implementations.

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Note**: While Simplex noise was historically patent-encumbered (patents expired in 2022), it remains an excellent choice for performance-critical applications. For projects requiring guaranteed patent-free status or targeting older codebases, consider OpenSimplex2 instead.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseSimplex.h)
