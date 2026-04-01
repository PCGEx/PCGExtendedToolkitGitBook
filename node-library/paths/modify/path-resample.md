---
description: Resample path to enforce equally spaced points.
icon: circle
---

# Path : Resample

### Overview

This node creates uniformly distributed points along a path by either sweeping at fixed intervals or redistributing existing points. New point positions are interpolated along the original path, with attributes blended from neighboring source points.

### How It Works

1. **Calculate Path Length**: Measure the total length of the path.
2. **Determine Sample Positions**: Based on mode, calculate where new points should be placed.
3. **Sample Points**: Create new points at the computed positions along the path.
4. **Blend Attributes**: Interpolate attributes from neighboring original points to new sample points.
5. **Assign Seeds**: Optionally ensure each new point has a unique random seed.

**Usage Notes**

* **Sweep vs Redistribute**: Sweep generates new points at fixed intervals (changing point count). Redistribute keeps the same point count but moves points to be evenly spaced.
* **Distance vs Count**: In Distance mode, you specify the spacing between points. In Count mode, you specify how many points you want.
* **Closed Loops**: For closed loops, "Preserve Last Point" is ignored since the path wraps around.
* **Blending**: Attributes on new points are interpolated from the two original points on either side of each sample position.

### Behavior

```
Resample (Sweep mode, distance-based):

Original (uneven spacing):
●───●─●───────●──●───●

After Resample (equal spacing):
●────●────●────●────●────●
     ↑ evenly distributed

Resample (Redistribute mode):
Same number of points, but evenly spaced along path length.
```

### Inputs

| Pin    | Type   | Description             |
| ------ | ------ | ----------------------- |
| **In** | Points | Path points to resample |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExResampleMode</code></summary>

How the path is resampled.

| Option           | Description                                                                          |
| ---------------- | ------------------------------------------------------------------------------------ |
| **Sweep**        | Walk along the path at fixed intervals, creating new points. May change point count. |
| **Redistribute** | Keep the same number of points but redistribute them evenly along the path.          |

Default: `Sweep`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Resolution Mode</strong> <code>EPCGExResolutionMode</code></summary>

How the sample spacing is determined.

| Option       | Description                           |
| ------------ | ------------------------------------- |
| **Distance** | Specify the distance between samples. |
| **Count**    | Specify the total number of samples.  |

Default: `Distance`

📋 _Visible when Mode = Sweep_

</details>

<details>

<summary><strong>Redistribute Evenly</strong> <code>bool</code></summary>

Adjust sample spacing so all samples are evenly distributed across the entire path length. When disabled, the last segment may be shorter than the target distance.

Default: `true`

📋 _Visible when Mode = Sweep and Resolution Mode = Distance_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve Last Point</strong> <code>bool</code></summary>

When redistribute evenly is disabled, this ensures the last original point is always included (even if it means a shorter final segment). Ignored for closed loops.

Default: `false`

📋 _Visible when Redistribute Evenly = false, Mode = Sweep, and Resolution Mode = Distance_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Resolution</strong> <code>double</code></summary>

The sampling resolution. In Distance mode, this is the spacing between points. In Count mode, this is the number of points to create.

Can be read from an attribute on the data domain.

Default: `100`

📋 _Visible when Mode = Sweep_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Truncate</strong> <code>EPCGExTruncateMode</code></summary>

How to handle fractional sample counts when the path length doesn't divide evenly by the sample distance.

| Option    | Description                                       |
| --------- | ------------------------------------------------- |
| **None**  | No truncation, may result in uneven last segment. |
| **Round** | Round to nearest whole number.                    |
| **Ceil**  | Round up (more samples, shorter spacing).         |
| **Floor** | Round down (fewer samples, longer spacing).       |

Default: `Round`

📋 _Visible when Mode = Sweep_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blending Settings</strong> <code>FPCGExBlendingDetails</code></summary>

Controls how attributes are interpolated from original path points to new sample points.

Default: Lerp blending for properties, None for attributes.

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Ensure Unique Seeds</strong> <code>bool</code></summary>

Generate unique random seeds for each resampled point. Ensures deterministic but varied randomness for downstream operations.

Default: `true`

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathResample.h)
