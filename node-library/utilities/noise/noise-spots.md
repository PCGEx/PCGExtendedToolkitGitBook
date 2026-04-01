---
description: Spots noise - circular/shaped spot patterns.
icon: circle-dashed
---

# Noise : Spots

### Overview

This noise generates discrete spot patterns by placing shaped regions (circles, squares, diamonds, stars) at jittered positions within a cellular grid. Unlike continuous gradient noises, Spots noise creates hard-edged or soft-falloff regions, making it ideal for stylized textures like polka dots, animal fur patterns (leopard spots, dalmatian), cellular structures, or any pattern requiring distinct, separated features. The spots can be inverted to create holes or gaps instead of raised regions.

### How It Works

1. **Grid Setup**: Divides 3D space into a regular cubic grid
2. **Spot Placement**: For each grid cell, determines a spot center position:
   * Base position at cell center
   * Applies Jitter to randomly offset within the cell
3. **Spot Sizing**: Calculates spot radius for each cell:
   * Starts with SpotRadius (relative to cell size)
   * Adds random RadiusVariation
4. **Shape Evaluation**: For the sample point, finds nearby grid cells and computes distance to their spot centers
5. **Shape Function**: Applies the selected shape function (circle, square, diamond, star) to determine if the point is inside the spot
6. **Value Calculation**: Assigns values based on proximity and shape:
   * Hard shapes: Binary inside/outside
   * Soft Circle: Smooth falloff from center to edge
   * Optional ValueVariation adds per-spot randomness
7. **Inversion**: If bInvertSpots is enabled, flips the pattern (spots become holes)
8. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Shape Options:

Circle (Hard):  ●  ●  ●  (sharp edges)
Circle (Soft):  ◉  ◉  ◉  (smooth falloff)
Square:         ■  ■  ■  (rectangular spots)
Diamond:        ◆  ◆  ◆  (rotated squares)
Star:           ★  ★  ★  (pointed patterns)
```

```
Spot Radius Effect:

Small (0.2):  · · · · · ·  (tiny dots, lots of space)
Medium (0.4): ● ● ● ● ● ●  (balanced spots)
Large (0.7):  ●●●●●●●●●●  (large spots, minimal gaps)
```

```
Jitter Effect:

None (0.0):   ● ● ● ● ● ●  (perfect grid alignment)
Low (0.2):    ●● ● ●● ●    (slight randomness)
High (0.5):   ● ●  ● ●  ●  (organic, irregular)
```

```
Inverted:

Normal:   ●  ●  ●  (white spots on black)
Inverted: ○  ○  ○  (black holes on white)
```

```
Radius Variation:

None (0.0):  ● ● ● ● ●  (uniform size)
High (0.3):  ● ● ● ● ●  (size variety)
             (varying radii create organic feel)
```

Good for: polka dots, leopard spots, dalmatian patterns, cell structures, stylized textures, decorative patterns

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Shape</strong> <code>EPCGExSpotsShape</code></summary>

Determines the geometric shape of each spot.

| Option                              | Description                                                     |
| ----------------------------------- | --------------------------------------------------------------- |
| **Circle (Hard)**                   | Sharp-edged circular spots with binary inside/outside           |
| **Circle (Soft Falloff)** (default) | Circular spots with smooth gradient falloff from center to edge |
| **Square**                          | Rectangular/square spots aligned with grid axes                 |
| **Diamond**                         | Diamond-shaped spots (45° rotated squares)                      |
| **Star**                            | Star-shaped spots with pointed edges                            |

Default: `Circle (Soft Falloff)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spot Radius</strong> <code>double</code></summary>

Base radius of each spot, specified as a fraction of the cell size (0-1 range). Controls how large the spots are relative to their grid spacing.

* **0.1**: Very small dots with large gaps
* **0.4** (default): Balanced spot-to-gap ratio
* **0.7**: Large spots that nearly touch or overlap

Values above 0.5 may cause spots to overlap depending on jitter.

Default: `0.4`

Range: `0.1` to `0.8`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Radius Variation</strong> <code>double</code></summary>

Random variation applied to spot radius. Adds organic irregularity by making some spots larger and others smaller.

* **0.0**: All spots same size
* **0.1** (default): Subtle size variation
* **0.3**: High variation, very organic

Variation is per-spot, seeded by cell position for consistency.

Default: `0.1`

Range: `0.0` to `0.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Jitter</strong> <code>double</code></summary>

Amount of random offset applied to spot positions within their grid cells. Higher jitter creates more irregular, natural-looking patterns.

* **0.0**: Perfect grid alignment (mechanical, regular)
* **0.3** (default): Natural organic variation
* **0.5**: Maximum irregularity

Jitter is deterministic per cell for stable patterns.

Default: `0.3`

Range: `0.0` to `0.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Spots</strong> <code>bool</code></summary>

When enabled, inverts the spot pattern, creating holes or gaps where spots would normally be, and filling the background.

* **false** (default): Spots are bright/raised on dark background
* **true**: Spots become dark holes on bright background

Useful for creating cutout patterns, voids, or negative space effects.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Value Variation</strong> <code>double</code></summary>

Random variation in the output value for each spot. Allows spots to have different brightness/intensity levels rather than all being the same value.

* **0.0** (default): All spots same intensity
* **0.5**: Moderate brightness variation
* **1.0**: Maximum variation (some spots very bright, others dim)

Adds visual complexity and prevents uniformity.

Default: `0.0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: Use the Frequency setting to control spot density (higher frequency = more, smaller spots), and combine with the bInvert base setting for double-inversion effects.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseSpots.h)
