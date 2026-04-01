---
description: >-
  Samples the field using six points around the sampling target location, and
  averaging the results.
icon: function
---

# Tensor Sampler (Six Points)

### Overview

The Six Points tensor sampler takes six samples arranged along the principal axes around the target location, then averages the results. By sampling in all cardinal directions (forward, backward, up, down, left, right), this method smooths out local noise and discontinuities in the tensor field, producing more stable directions in turbulent or noisy regions.

### How It Works

1. **Sample Positions**: Calculates six sample positions offset from the target along each axis direction.
2. **Multi-Sampling**: Samples the tensor field at all six positions.
3. **Averaging**: Combines the six samples to produce a smoothed direction vector.
4. **Output**: Returns the averaged tensor direction for use in extrusion or other operations.

**Usage Notes**

* **Noise Smoothing**: Effective at reducing jitter in noisy tensor fields where single-point sampling would produce erratic results.
* **Axis-Aligned Sampling**: Uses the world axes (Forward, Backward, Up, Down, Left, Right), which may not align optimally with all field orientations.
* **Performance**: Six samples per step means higher cost than single-point sampling, but provides better stability.

### Behavior

```
Six-Point Sampling Pattern:

              ↑ Up
              │
              ●
              │
    Left ●────●────● Right
              │
              ●
              │
              ↓ Down

    (Plus Forward/Backward on Z-axis)

Each ● is a sample point offset from center.
Final direction = average of all six samples.

Result: Smoothed direction that filters out
        local noise and discontinuities.
```

### Settings

This sampler inherits all settings from the base tensor sampler.

#### Inherited Settings

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base radius for tensor sampling operations. Controls the offset distance for the six sample points.

Default: `1`

</details>

<details>

<summary><strong>Min Step Fraction</strong> <code>double</code></summary>

Minimum step size as a fraction of the base radius.

Default: `0.1`

Range: `0.01` to `1.0`

</details>

<details>

<summary><strong>Max Step Fraction</strong> <code>double</code></summary>

Maximum step size as a fraction of the base radius.

Default: `1.0`

Range: `0.1` to `2.0`

</details>

<details>

<summary><strong>Error Tolerance</strong> <code>double</code></summary>

Error tolerance for sampling accuracy.

Default: `0.01`

Range: `0.001` to `0.5`

</details>

<details>

<summary><strong>Max Sub Steps</strong> <code>int32</code></summary>

Maximum number of sub-steps per sample for improved accuracy.

Default: `4`

Range: `1` to `16`

</details>

→ See Tensor Sampler (Default) for complete inherited settings documentation.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Samplers/PCGExTensorSamplerSixPoints.h)
