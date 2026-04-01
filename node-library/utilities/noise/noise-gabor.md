---
description: Gabor noise - directional/anisotropic patterns.
icon: circle-dashed
---

# Noise : Gabor

### Overview

This noise generates anisotropic (directional) patterns by using Gabor kernels — sinusoidal waves modulated by Gaussian envelopes. Unlike most noise types which are isotropic (the same in all directions), Gabor noise has a controllable direction, making it ideal for creating oriented patterns like wood grain flowing along growth rings, brushed metal surfaces, fabric weaves, or any pattern that follows a specific direction.

### How It Works

1. **Cell Grid Setup**: Divides space into a grid and places random impulses within each cell
2. **Gabor Kernel Evaluation**: For each impulse, evaluates a Gabor kernel consisting of:
   * **Gaussian Envelope**: Controls the spatial extent and falloff
   * **Sinusoidal Carrier**: Creates the wave pattern along the Direction vector
3. **Impulse Accumulation**: Combines contributions from ImpulsesPerCell random impulses per grid cell
4. **Bandwidth Control**: Lower bandwidth makes the pattern more directional (tight along Direction), higher bandwidth makes it more isotropic
5. **Kernel Radius**: Limits the spatial influence of each impulse for performance
6. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Direction Effect:

Direction = (1, 0, 0): |||||||  (horizontal stripes/grain)
Direction = (0, 1, 0): ═══════  (vertical stripes/grain)
Direction = (1, 1, 0): ///////  (diagonal stripes)
```

```
Bandwidth Effect:

Low (0.1):  |||||||||||  (very directional, tight grain)
Med (1.0):  ||| ||| |||  (moderate directionality)
High (10.0): ··········  (nearly isotropic, loses direction)
```

```
Impulses Per Cell:

1-4:   Sparse, irregular pattern
8:     Natural, organic variation (default)
32:    Dense, uniform pattern
```

```
Kernel Radius:

Small (0.5): ·|·|·|·|·  (tight, fine details)
Medium (1.5): ||||||||| (natural, connected)
Large (4.0):  ▓▓▓▓▓▓▓▓▓ (broad, smooth)
```

Good for: wood grain, fabric, brushed metal, directional patterns, anisotropic textures

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Direction</strong> <code>FVector</code></summary>

The direction vector along which the Gabor pattern flows. This vector is automatically normalized, so only the direction matters, not the length.

Examples:

* **(1, 0, 0)**: Pattern flows along X-axis (horizontal grain)
* **(0, 1, 0)**: Pattern flows along Y-axis (vertical grain)
* **(0, 0, 1)**: Pattern flows along Z-axis
* **(1, 1, 0)**: 45° diagonal in XY plane

Use with Apply Transform to rotate or orient the pattern in world space.

Default: `(1, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bandwidth</strong> <code>double</code></summary>

Controls how tightly concentrated the pattern is along the Direction vector. Lower values create sharper, more directional patterns, while higher values make the pattern more spread out and isotropic (losing directionality).

* **0.1**: Very directional, sharp grain lines
* **1.0** (default): Natural directional character
* **10.0**: Almost isotropic, diffuse pattern

Default: `1.0`

Range: `0.1` to `10.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Impulses Per Cell</strong> <code>int32</code></summary>

Number of random Gabor kernel impulses placed within each grid cell. More impulses create denser, more uniform patterns, while fewer impulses create sparser, more irregular patterns.

* **1-4**: Sparse, irregular (wood knots, fabric irregularities)
* **8** (default): Natural organic variation
* **16-32**: Dense, uniform (fine fabric weave, smooth metal)

Each impulse increases computation cost linearly.

Default: `8`

Range: `1` to `32`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Kernel Radius</strong> <code>double</code></summary>

Spatial extent of each Gabor kernel's influence. Larger radii create smoother, more connected patterns, while smaller radii create tighter, finer details.

* **0.5**: Very tight, fine grain
* **1.5** (default): Natural feature size
* **4.0**: Broad, smooth patterns

Larger radii evaluate more cells per sample, increasing computation cost.

Default: `1.5`

Range: `0.5` to `4.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: Use the Transform setting to rotate the entire pattern in 3D space, or modify the Direction vector to change the flow orientation dynamically.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseGabor.h)
