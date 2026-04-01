---
description: Classic Perlin gradient noise.
icon: circle-dashed
---

# Noise : Perlin

### Overview

This noise implements classic Perlin noise, the original gradient noise algorithm created by Ken Perlin in 1983 that revolutionized computer graphics. It generates smooth, continuous, pseudo-random patterns by interpolating between random gradients on a regular grid. While newer algorithms like OpenSimplex2 offer some improvements, Perlin noise remains a fundamental and widely-used tool for procedural generation, known for its reliability and well-understood characteristics.

### How It Works

1. **Grid Setup**: Establishes a regular cubic grid with gradient vectors at each lattice point
2. **Cell Identification**: Determines which grid cell contains the sample point
3. **Gradient Lookup**: Retrieves pseudo-random gradient vectors for the 8 corners of the cell
4. **Dot Products**: Computes dot products between each corner's gradient and the vector from that corner to the sample point
5. **Interpolation**: Blends the 8 contributions using smooth interpolation (ease curve), typically using Perlin's improved smoothstep function (6t⁵ - 15t⁴ + 10t³)
6. **Octave Layering**: Optionally combines multiple octaves at different frequencies for fractal detail
7. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Characteristics:

- Smooth, continuous gradients
- Regular grid structure (can show axis-aligned artifacts)
- Well-understood, predictable behavior
- Classic "cloudy" appearance
```

```
Octaves Effect:

1 Octave:  ~~~~~~~~~~~~  (smooth, large features)
4 Octaves: ≈≈≈≈≈≈≈≈≈≈≈≈  (good detail, natural)
16 Octaves: ::::::::::::  (very detailed, noisy)
```

```
Potential Artifacts:

- May show slight axis-aligned patterns on close inspection
- Grid-based structure can be visible in certain applications
- Generally less isotropic than OpenSimplex2
```

```
Comparison:

Perlin:        Classic, reliable, slight grid artifacts
OpenSimplex2:  Better isotropy, patent-free
Simplex:       Faster, patent concerns
```

Good for: general-purpose noise, terrain, textures, clouds, any classic procedural effects

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

**Note**: While Perlin noise can show minor grid artifacts, it remains an excellent choice for most applications due to its well-understood behavior and widespread use. For applications where perfect isotropy is critical, consider OpenSimplex2 instead.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoisePerlin.h)
