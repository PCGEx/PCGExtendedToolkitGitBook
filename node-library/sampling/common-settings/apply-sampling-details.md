---
description: >-
  Configures how transform data from sampling operations is applied to the
  source points.
icon: sliders-simple
---

# Apply Sampling Details

### Overview

This settings block controls how sampled transform information affects the points that performed the sampling. When sampling nearest surfaces, bounds, or other targets, this determines which components (position, rotation, scale) of the sampled transform are applied back to the source points, and on which axes. Each component can be selectively enabled per-axis.

### How It Works

1. **Sample Target**: A point samples a target (surface, bounds, etc.) and receives transform data
2. **Select Components**: Choose which transform components to apply (position, rotation, scale)
3. **Select Axes**: For each component, select which axes (X, Y, Z) to apply
4. **Modify Points**: Source points are updated with the selected sampled values

### Behavior

```
Sampling Application Example:

Sampled Transform:
  Position: (100, 50, 25)
  Rotation: (0, 45°, 0)
  Scale:    (2, 2, 2)

Original Point:
  Position: (0, 0, 0)
  Rotation: (0, 0, 0)
  Scale:    (1, 1, 1)

Settings: Apply Transform Position (Y, Z only)
Result:
  Position: (0, 50, 25)  ← X unchanged, Y and Z from sample
  Rotation: (0, 0, 0)    ← unchanged
  Scale:    (1, 1, 1)    ← unchanged
```

### Settings

<details>

<summary><strong>Apply Transform</strong> <code>bool</code></summary>

Enables application of sampled transform components to points.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Position</strong> <code>bitmask</code></summary>

Selects which position axes (X, Y, Z) from the sampled transform are applied to point positions.

📋 _Visible when Apply Transform = true_

</details>

<details>

<summary><strong>Rotation</strong> <code>bitmask</code></summary>

Selects which rotation components from the sampled transform are applied to point rotations.

📋 _Visible when Apply Transform = true_

</details>

<details>

<summary><strong>Scale</strong> <code>bitmask</code></summary>

Selects which scale axes (X, Y, Z) from the sampled transform are applied to point scales.

📋 _Visible when Apply Transform = true_

</details>

<details>

<summary><strong>Apply Look At</strong> <code>bool</code></summary>

Enables application of look-at rotation, orienting points to face toward the sampled target.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Look At Rotation</strong> <code>bitmask</code></summary>

Selects which rotation components from the look-at calculation are applied to point rotations.

📋 _Visible when Apply Look At = true_

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Sampling/PCGExApplySamplingDetails.h)
