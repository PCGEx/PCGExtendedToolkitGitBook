---
description: Voronoi noise - cell patterns with multiple modes.
icon: circle-dashed
---

# Noise : Voronoi

### Overview

This noise generates cellular patterns based on Voronoi diagrams (also called Worley noise in some contexts). It divides 3D space into cells, each containing a randomly positioned feature point. The noise value at any location is determined by the relationship to these feature points — either the cell's random value, the distance to the nearest point, the distance to cell boundaries, or the difference between distances to the first and second nearest points. This versatility makes Voronoi noise ideal for organic cellular textures, stone patterns, cracked surfaces, and procedural partitioning.

### How It Works

1. **Grid Setup**: Divides space into a cubic grid of cells
2. **Feature Point Placement**: Places a random feature point within each cell:
   * Jitter controls how randomly positioned (0 = centered, 1 = fully random)
3. **Neighbor Search**: For each sample point, examines nearby grid cells to find the closest feature points
4. **Distance Calculation**: Computes Euclidean distances to the nearest (F1) and second-nearest (F2) feature points
5. **Output Mode Application**: Generates the final value based on OutputType:
   * **Cell Value**: Returns a random value associated with the nearest cell (flat regions, each cell has uniform value)
   * **Distance to Center**: Returns the distance to F1 (creates gradients radiating from cell centers)
   * **Edge Distance**: Returns distance to nearest cell boundary (highlights edges where cells meet)
   * **Crackle (F2-F1)**: Returns the difference between F2 and F1 distances (emphasizes cell boundaries, creates cracked appearance)
6. **Smoothness**: Optionally blends between cells using smooth minimum function for softer transitions
7. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Output Type Effects:

Cell Value:          ▓▓▓▓▒▒▒░░░░  (flat regions, discrete values)
  Each cell has a uniform random value
  Creates clear cell boundaries

Distance to Center:  ···●●●●●···  (radial gradients)
  Dark at cell centers, bright at edges
  Circular/spherical falloff from feature points

Edge Distance:       ████│││││    (highlighted boundaries)
  Bright in cell interiors
  Dark at cell edges/boundaries

Crackle (F2-F1):     ████┃┃┃┃┃    (emphasized cracks)
  Very low values at cell boundaries
  Creates crack-like patterns between cells
```

```
Jitter Effect:

None (0.0):   ████████  (perfect grid, regular cells)
Low (0.5):    ███▓█▓██  (slight irregularity)
Full (1.0):   █▓▒░▒▓█▓  (organic, irregular cells)
```

```
Smoothness Effect (with Distance mode):

None (0.0):   ●●●│●●●│  (hard boundaries)
Low (0.3):    ●●●╱●●●╲  (slightly soft)
High (0.8):   ●●●~●●●~  (very smooth blending)
```

Good for: cellular textures, stone patterns, cracked surfaces, organic partitions, procedural terrain features, cell-based effects

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Output Type</strong> <code>EPCGExVoronoiOutput</code></summary>

Determines what value the noise returns based on the Voronoi cell structure.

| Option                   | Description                                                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cell Value** (default) | Returns a random value for each cell, creating flat regions with discrete boundaries. Each cell has a uniform value.                                                |
| **Distance to Center**   | Returns the distance from the sample point to the nearest feature point (F1), creating radial gradients emanating from cell centers.                                |
| **Edge Distance**        | Returns the distance to the nearest cell edge/boundary, useful for highlighting or masking cell borders.                                                            |
| **Crackle (F2-F1)**      | Returns the difference between distances to the second-nearest (F2) and nearest (F1) feature points, creating emphasized crack-like patterns along cell boundaries. |

Default: `Cell Value`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Jitter</strong> <code>double</code></summary>

Controls how much the feature points are randomly offset within their cells. Lower jitter creates more regular, grid-like patterns, while higher jitter produces organic, irregular cells.

* **0.0**: No jitter, feature points at cell centers (perfect grid, regular cells)
* **0.5**: Moderate randomization
* **1.0** (default): Full randomization, feature points anywhere in cell (organic, natural)

Jitter affects cell shape and size distribution, with higher values creating more varied cell patterns.

Default: `1.0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smoothness</strong> <code>double</code></summary>

Amount of smooth blending between cells when using distance-based output modes. Higher values create softer transitions instead of hard boundaries.

* **0.0** (default): No smoothing, hard cell boundaries
* **0.3**: Subtle softening of edges
* **0.8**: Very smooth, blended transitions

Uses a smooth minimum function to blend distances from multiple feature points. Most effective with Distance to Center and Crackle modes.

Default: `0.0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Tip**: Different output modes work well for different use cases:

* **Cell Value**: Flat cellular textures, region partitioning, random per-cell effects
* **Distance to Center**: Bubble-like patterns, radial effects, spot highlights
* **Edge Distance**: Cell boundary detection, masking edges, wireframe effects
* **Crackle**: Cracked surfaces, dried mud, shattered glass, geological fissures

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseVoronoi.h)
