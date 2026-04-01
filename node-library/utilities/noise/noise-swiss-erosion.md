---
description: Swiss noise - terrain with natural erosion patterns.
icon: circle-dashed
---

# Noise : Swiss (Erosion)

### Overview

This noise generates terrain-like patterns with natural erosion features by using analytical derivatives to warp and erode the noise function itself. Also known as "erosion noise" or "Swiss turbulence," it creates organic ridges, valleys, and channels that resemble geological weathering processes — as if water has carved paths through the terrain over time. The technique uses the slope information (derivatives) of the noise to influence the pattern's evolution, producing sharper ridges and deeper valleys than standard fractal noise.

### How It Works

1. **Base Noise with Derivatives**: Computes Perlin noise along with its analytical derivatives (the gradient/slope at each point)
2. **Derivative Warping**: Uses the derivative information to warp subsequent octaves:
   * The slope direction guides how the pattern is distorted
   * WarpFactor controls the intensity of this warping
3. **Erosion Application**: Applies ErosionStrength to modify amplitude based on derivatives:
   * High slope areas (ridges) get enhanced
   * Low slope areas (valleys) get deepened
   * Creates the characteristic carved appearance
4. **Octave Accumulation**: Unlike standard fBm, each octave is warped by the derivatives of previous octaves, creating compound erosion effects
5. **Ridge Formation**: The derivative feedback creates sharp ridges where erosion carves deep channels
6. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Characteristics:

- Sharp ridges and deep valleys
- Natural-looking erosion channels
- Organic, terrain-like features
- More structured than standard fBm
```

```
Erosion Strength Effect:

None (0.0):    ~~~~~~~~~~~~  (standard fBm, smooth)
Low (0.4):     ~~~∼∼∼~~~∼∼  (gentle erosion hints)
Medium (0.8):  ∼∼^∼∼^∼∼^∼  (clear ridges and valleys)
High (1.5):    ^V^V^V^V^V^  (very sharp, dramatic erosion)
```

```
Warp Factor Effect:

None (0.0):    Straight erosion patterns
Low (0.15):    Gentle meandering (natural)
High (0.5):    Highly twisted, chaotic channels
```

```
Octaves (note default is 6):

3 Octaves:  Simple terrain features
6 Octaves:  Natural, detailed terrain (default)
12 Octaves: Highly complex, intricate erosion
```

```
Comparison:

Standard fBm:   ~~~~~~~~~~~~  (smooth hills)
Ridged:         ^^^^^^^^^^^^  (sharp peaks)
Swiss:          ^∼V∼^∼V∼^∼V  (peaks + carved valleys)
```

Good for: terrain generation, canyon systems, eroded landscapes, geological features, natural cliff formations

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine. Each octave adds finer erosion detail at progressively higher frequency.

* **3**: Simple terrain with basic erosion features
* **6** (default): Natural, well-detailed terrain
* **12**: Highly complex erosion patterns

More octaves create more intricate channel networks and erosion detail. Note the higher default (6 instead of 1) compared to other noises, as Swiss noise benefits from multiple octaves to show its characteristic erosion patterns.

Default: `6`

Range: `1` to `16`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lacunarity</strong> <code>double</code></summary>

Frequency multiplier between successive octaves. Controls how quickly erosion detail scales up with each additional layer.

* **1.5**: Gentle frequency progression
* **2.0** (default): Standard doubling per octave
* **4.0**: Aggressive frequency increase, creates finer channel networks

Default: `2.0`

Range: `1.0` to `4.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Persistence</strong> <code>double</code></summary>

Amplitude multiplier between successive octaves, controlling the contribution of each erosion layer to the final result.

* **0.2**: Smooth, large-scale terrain features dominate
* **0.5** (default): Balanced erosion across scales
* **0.9**: Rough, highly detailed small-scale erosion prominent

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Erosion Strength</strong> <code>double</code></summary>

Controls how much the noise derivatives (slopes) affect the erosion pattern. Higher values create more pronounced ridges and deeper valleys.

* **0.0**: No erosion effect (degrades to standard fBm)
* **0.8** (default): Natural erosion features
* **1.5**: Very dramatic, sharp ridges and deep channels
* **2.0**: Extreme erosion, highly stylized

The derivative feedback amplifies differences in slope, carving out valleys and sharpening peaks.

Default: `0.8`

Range: `0.0` to `2.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Warp Factor</strong> <code>double</code></summary>

Amount of derivative-based warping applied to distort the erosion patterns. Higher values create more meandering, twisted channels.

* **0.0**: No warping (straight erosion channels)
* **0.15** (default): Natural, organic meandering
* **0.5**: Heavy warping, chaotic flow patterns

Warping uses the gradient direction to offset sampling positions, creating flowing, river-like erosion paths rather than straight lines.

Default: `0.15`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: Swiss noise works particularly well with moderate to high octave counts (4-8) to fully develop its erosion character. For more dramatic terrain, increase ErosionStrength; for more organic flow, increase WarpFactor.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseSwiss.h)
