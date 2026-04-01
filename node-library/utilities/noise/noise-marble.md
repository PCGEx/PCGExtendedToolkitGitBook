---
description: Marble noise - veined patterns with turbulence.
icon: circle-dashed
---

# Noise : Marble

### Overview

This noise generates marble-like veined patterns by combining periodic sine waves with turbulent distortion. It creates the characteristic flowing, irregular stripes found in natural marble stone, wood grain, or geological formations. The base pattern is a series of parallel bands that are then warped and twisted by layered turbulence noise, producing organic, natural-looking variations.

### How It Works

1. **Base Pattern Selection**: Depending on Direction, establishes the initial orientation of veins:
   * **X/Y/Z Axis**: Planar veins perpendicular to the chosen axis
   * **Radial**: Concentric circular veins emanating from the origin
2. **Sine Wave Generation**: Creates periodic bands using a sine function at VeinFrequency
3. **Turbulence Generation**: Produces multi-octave noise for distortion
4. **Pattern Distortion**: Adds turbulence to the sine input, warping the straight bands into flowing veins
5. **Vein Sharpening**: Applies power function to sharpen or soften the vein edges
6. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Direction Modes:

X Axis:  |||||||  (veins perpendicular to X, YZ planes)
Y Axis:  ═══════  (veins perpendicular to Y, XZ planes)
Z Axis:  ▬▬▬▬▬▬▬  (veins perpendicular to Z, XY planes)
Radial:  ◯◯◯◯◯◯◯  (concentric rings from origin)
```

```
Vein Frequency:

Low (1.0):   |     |     |     |  (wide bands)
Med (5.0):   | | | | | | | | | |  (natural spacing)
High (20.0): |||||||||||||||||  (fine, tight veins)
```

```
Turbulence Strength:

None (0.0):  ||||||||  (straight, parallel lines)
Low (0.5):   ||~||~||  (gentle waviness)
Med (1.0):   |~≈~|~≈~  (organic marble flow)
High (5.0):  ≈~≈~≈~≈~  (chaotic, heavily distorted)
```

```
Vein Sharpness:

Soft (1.0):  ░▒▓▒░▒▓  (blurred transitions)
Med (3.0):   ▒▓█▓▒▓█  (clear veins)
Sharp (8.0): █ █ █ █  (crisp, defined edges)
```

Good for: marble textures, wood grain, geological patterns, stylized effects

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Direction</strong> <code>EPCGExMarbleDirection</code></summary>

Determines the orientation and geometry of the marble veins.

| Option     | Description                                                                       |
| ---------- | --------------------------------------------------------------------------------- |
| **X Axis** | Veins perpendicular to the X axis, forming YZ planar bands                        |
| **Y Axis** | Veins perpendicular to the Y axis, forming XZ planar bands                        |
| **Z Axis** | Veins perpendicular to the Z axis, forming XY planar bands                        |
| **Radial** | Concentric circular veins emanating from the origin, creating onion-ring patterns |

Default: `X Axis`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Vein Frequency</strong> <code>double</code></summary>

Controls how tightly spaced the marble veins are. Higher frequencies create finer, more numerous veins.

* **0.1**: Very wide, sparse bands
* **5.0** (default): Natural marble vein spacing
* **20.0**: Very fine, densely packed veins

Default: `5.0`

Range: `0.1` to `20.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Turbulence Strength</strong> <code>double</code></summary>

Amount of turbulent distortion applied to the veins. Higher values create more warping and organic flow.

* **0.0**: No distortion (straight parallel lines)
* **1.0** (default): Natural marble distortion
* **5.0**: Extreme warping, chaotic patterns

Default: `1.0`

Range: `0.0` to `5.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Turbulence Octaves</strong> <code>int32</code></summary>

Number of noise layers used to create the turbulence distortion. More octaves add finer detail to the warping.

* **1**: Simple, broad distortion
* **4** (default): Natural complexity
* **8**: Highly detailed, intricate warping

Each octave increases computation cost.

Default: `4`

Range: `1` to `8`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Vein Sharpness</strong> <code>double</code></summary>

Controls the edge definition of the marble veins. Higher values create sharper, more defined edges between light and dark bands.

* **1.0** (default): Soft, blended transitions (natural marble)
* **3.0**: Clear, defined veins
* **8.0**: Very sharp, high-contrast edges (stylized, crystalline)

Achieved by applying a power function to the sine output.

Default: `1.0`

Range: `1.0` to `8.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: Use the Transform setting to rotate the vein direction in 3D space, or combine multiple marble noises with different orientations for more complex patterns.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseMarble.h)
