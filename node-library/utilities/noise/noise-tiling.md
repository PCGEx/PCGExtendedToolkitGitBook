---
description: Tiling noise - seamlessly tileable patterns.
icon: circle-dashed
---

# Noise : Tiling

### Overview

This noise generates seamlessly tileable gradient noise that repeats perfectly along each axis. By using periodic hash wrapping at specified intervals (PeriodX, PeriodY, PeriodZ), it ensures the noise values at position 0 match those at position Period, creating patterns that tile without visible seams or discontinuities. This is essential for creating repeating textures for materials, backgrounds, or any application where the pattern must wrap seamlessly in 3D space.

### How It Works

1. **Period Setup**: Establishes tile periods for each axis (X, Y, Z), defining how many units before the pattern repeats
2. **Coordinate Wrapping**: Applies modulo operation to coordinates before hashing:
   * Position 0 wraps to the same hash as position Period
   * Positions Period-1 and 0 share gradient continuity
3. **Periodic Hashing**: Uses a custom hash function that wraps grid coordinates within the period bounds
4. **Gradient Interpolation**: Standard gradient noise interpolation, but with wrapped coordinates ensuring edge values match
5. **Octave Layering**: Each octave also tiles at the specified period (scaled by lacunarity)
6. **Seamless Boundaries**: The pattern at (PeriodX, y, z) is identical to (0, y, z), and similarly for Y and Z axes
7. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Tiling Demonstration (2D example):

Period = 4 units:
  0  1  2  3  0  1  2  3  (seamless repeat)
  ~~~∼∼∼~~~∼∼∼~~~∼∼∼~~~  (pattern tiles perfectly)

Non-tiling noise:
  0  1  2  3  4  5  6  7
  ~~~∼∼∼≈≈≈^^^^vvvv∼∼∼  (no guarantee of match)
          ↑ visible seam when tiled
```

```
Period Size Effect:

Small (2):   Frequent repetition, pattern visible
Medium (4):  Good balance, natural appearance
Large (32):  Subtle tiling, nearly unique appearance
```

```
Independent Axis Periods:

PeriodX=8, PeriodY=8, PeriodZ=2:
Creates patterns that tile every 8 units in X and Y,
but only every 2 units in Z (useful for layered effects)
```

```
Octaves Impact:

More octaves add detail while maintaining tileability.
All octave layers tile at the base period.
```

Good for: tileable textures, seamless backgrounds, repeating material patterns, texture atlases, wrapping 3D spaces

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine for fractal detail. Each octave adds finer detail at progressively higher frequency, while maintaining tileability.

* **1** (default): Single layer, smooth base noise
* **4**: Good detail balance
* **16**: Maximum detail

Each octave approximately doubles computation cost. All octaves tile at the same period.

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

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Period X</strong> <code>int32</code></summary>

Number of units before the pattern repeats along the X axis. The noise value at position X will match the noise value at position X + PeriodX.

* **2**: Very frequent repetition
* **4** (default): Moderate tiling
* **64**: Large tile, less obvious repetition

Smaller periods create more noticeable repetition but use less memory for precomputation. Larger periods appear more random but still tile perfectly.

Default: `4`

Range: `1` to `256`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Period Y</strong> <code>int32</code></summary>

Number of units before the pattern repeats along the Y axis. Works identically to Period X but controls Y-axis tiling.

* **2**: Very frequent repetition
* **4** (default): Moderate tiling
* **64**: Large tile

Can differ from Period X to create non-square tiling patterns.

Default: `4`

Range: `1` to `256`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Period Z</strong> <code>int32</code></summary>

Number of units before the pattern repeats along the Z axis. Controls tiling in the third dimension.

* **2**: Very frequent repetition
* **4** (default): Moderate tiling
* **64**: Large tile

Can be set differently from X and Y for anisotropic tiling (e.g., tiling every 2 units vertically but 16 horizontally).

Default: `4`

Range: `1` to `256`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: When using tiling noise, consider the Frequency setting in relation to the period. If Frequency is very high relative to the period, the tiling will become more apparent. For best results, keep the period large enough to contain several frequency cycles (Period × Frequency should generally be > 1).

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseTiling.h)
