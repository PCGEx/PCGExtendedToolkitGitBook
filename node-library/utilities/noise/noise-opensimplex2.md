---
description: OpenSimplex2 - patent-free, high quality gradient noise.
icon: circle-dashed
---

# Noise : OpenSimplex2

### Overview

This noise implements the OpenSimplex2 algorithm, a patent-free gradient noise function that serves as an improved alternative to classic Simplex noise. It offers better visual isotropy (more even distribution in all directions) than traditional Simplex noise while maintaining computational efficiency. OpenSimplex2 is ideal for general-purpose procedural noise applications where smooth, natural-looking patterns are needed without directional artifacts.

### How It Works

1. **Coordinate Transformation**: Applies a squish/stretch transformation to the input coordinates to map them onto a skewed grid
2. **Simplex Cell Identification**: Determines which simplex cell (4-cornered tetrahedron in 3D) contains the sample point
3. **Gradient Selection**: Retrieves pre-computed gradient vectors from a carefully designed set of 24 gradients on the vertices of a rhombic dodecahedron
4. **Contribution Calculation**: Calculates the contribution from each simplex vertex using:
   * Distance-based attenuation (smooth falloff)
   * Dot product with gradient vector
5. **Octave Layering**: Optionally combines multiple octaves at different frequencies for fractal detail
6. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Characteristics:

- Smooth, continuous gradients
- No directional bias (isotropic)
- Good frequency response across all scales
- Organic, natural appearance
```

```
Octaves Effect:

1 Octave:  ~~~~~~~~~~~~  (smooth, large features)
4 Octaves: ≈≈≈≈≈≈≈≈≈≈≈≈  (good detail)
16 Octaves: ::::::::::::  (extremely detailed)
```

```
Lacunarity & Persistence (same as other fractal noises):

Lacunarity 2.0 + Persistence 0.5 = Natural fractal distribution
Lacunarity 3.0 + Persistence 0.7 = More chaotic, rougher
Lacunarity 1.5 + Persistence 0.3 = Smoother, gentler
```

```
Comparison to Other Gradient Noises:

Perlin:        Good but can show grid artifacts
Simplex:       Fast but has patent concerns, slight directional bias
OpenSimplex2:  Patent-free, better isotropy, high quality
```

Good for: general-purpose noise, terrain, textures, organic patterns, any application needing smooth gradient noise

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
* **2.0** (default): Standard doubling per octave
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

**Note**: OpenSimplex2 is an excellent default choice for gradient noise. Its improved isotropy means it won't create unintended directional patterns, and its patent-free status makes it safe for all applications.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseOpenSimplex2.h)
