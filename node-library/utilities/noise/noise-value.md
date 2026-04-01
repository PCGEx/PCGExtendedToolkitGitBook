---
description: Value noise - fast, interpolated random values.
icon: circle-dashed
---

# Noise : Value

### Overview

This noise generates patterns by interpolating between random values assigned to a regular grid of lattice points. Unlike gradient noises (Perlin, Simplex) which use directional gradients, Value noise simply places a random scalar value at each grid point and smoothly blends between them. This makes it computationally faster than gradient-based approaches, though it can produce a more "blocky" or artificial appearance with visible grid alignment. Value noise is ideal when performance is critical and a slightly more structured look is acceptable.

### How It Works

1. **Grid Setup**: Establishes a regular cubic lattice in 3D space
2. **Value Assignment**: Each lattice point gets a pseudo-random value (0-1 range) based on its coordinates and the seed
3. **Cell Identification**: Determines which grid cell contains the sample point
4. **Corner Value Lookup**: Retrieves the random values at the 8 corners of the containing cube
5. **Trilinear Interpolation**: Smoothly blends the 8 corner values using a smoothstep interpolation function:
   * Interpolates along X between the 4 edges parallel to X
   * Interpolates along Y between the 2 resulting edges
   * Final interpolation along Z yields the output value
6. **Octave Layering**: Optionally combines multiple octaves at different frequencies for fractal detail
7. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Characteristics:

- Fast computation (simpler than gradient noise)
- Smooth interpolation between random values
- Can show grid-aligned patterns (more "blocky")
- Less organic than gradient noises
- Good frequency response
```

```
Octaves Effect:

1 Octave:  ~~~~~~~~~~~~  (smooth, large features)
4 Octaves: ≈≈≈≈≈≈≈≈≈≈≈≈  (good detail)
16 Octaves: ::::::::::::  (very detailed)
```

```
Visual Comparison:

Value Noise:     More regular, slight grid feel
Gradient Noise:  More organic, flowing appearance
```

```
Performance:

Value noise is faster than Perlin/Simplex because:
- No gradient computations (just random value lookups)
- Simpler per-point calculations
- Fewer mathematical operations
```

```
Artifacts:

- May show subtle grid alignment
- Less directional variety than gradient noise
- Can appear more "artificial" in some contexts
```

Good for: performance-critical applications, stylized effects, fast procedural textures, background patterns

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine for fractal detail. Each octave adds finer detail at progressively higher frequency.

* **1** (default): Single layer, smooth base noise
* **4**: Good balance of detail and performance
* **8**: High detail
* **16**: Maximum detail (expensive)

Each octave approximately doubles computation cost, though Value noise remains faster than gradient alternatives at the same octave count.

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

**Note**: Value noise is an excellent choice when computation speed is important and the slightly more structured appearance is acceptable. For more organic, natural patterns, consider gradient-based noises like OpenSimplex2 or Perlin instead.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseValue.h)
