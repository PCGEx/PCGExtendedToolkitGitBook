---
description: Find the closest transform on nearest polylines.
icon: circle
---

# Sample : Nearest Spline

### Overview

This node samples data from nearby spline curves, finding the closest points along splines and computing weighted transforms, distances, tangents, and other spatial relationships. It can sample at the closest point on each spline or at a specific alpha position, making it useful for aligning objects to curves or computing distance fields from splines.

### How It Works

1. **Find Splines**: Locates splines within range of each source point.
2. **Sample Position**: Finds closest point or samples at specific alpha.
3. **Compute Transform**: Gets position, rotation, and scale from spline.
4. **Blend Results**: Combines sampled data using distance weights.
5. **Write Outputs**: Stores transforms, distances, and tangents.

**Usage Notes**

* **Spline Types**: Works with closed loops, open splines, or both.
* **Alpha Sampling**: Can sample at specific position along spline.
* **Spline Scale**: Optionally uses spline scale to affect sampling range.
* **Tangent Data**: Can output tangent vectors for alignment.

### Behavior

**Spline Sampling:**

```
Source point P with splines in range:
   Spline1: closed loop, closest point at alpha 0.3
   Spline2: open curve, closest point at alpha 0.7

Sample Method = Closest:
   → Samples the nearest spline only

bSampleSpecificAlpha = true, Alpha = 0.5:
   → Samples both splines at their midpoint (alpha 0.5)
```

### Inputs

| Pin         | Type   | Description                        |
| ----------- | ------ | ---------------------------------- |
| **In**      | Points | Source points to sample from       |
| **Splines** | Spline | Target spline components to sample |

### Settings

#### Sampling

<details>

<summary><strong>Sample Inputs</strong> <code>EPCGExSplineSamplingIncludeMode</code></summary>

Which splines to include in sampling.

| Option                | Description                |
| --------------------- | -------------------------- |
| **All**               | Sample all splines         |
| **Closed Loops Only** | Only sample closed splines |
| **Open Splines Only** | Only sample open splines   |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Method</strong> <code>EPCGExSampleMethod</code></summary>

How to select which splines to sample.

| Option              | Description                     |
| ------------------- | ------------------------------- |
| **Within Range**    | Sample all splines within range |
| **Closest Target**  | Sample only the closest spline  |
| **Farthest Target** | Sample only the farthest spline |
| **Best Candidate**  | Sample based on sorting rules   |

Default: `Within Range`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spline Scales Ranges</strong> <code>bool</code></summary>

When enabled, spline scale affects the sampling range.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Range Min</strong> <code>double</code></summary>

Minimum sampling distance. Can be constant or from attribute.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Range Max</strong> <code>double</code></summary>

Maximum sampling distance. Can be constant or from attribute.

Default: `300`

⚡ PCG Overridable

</details>

#### Alpha Sampling

<details>

<summary><strong>Sample Specific Alpha</strong> <code>bool</code></summary>

When enabled, samples at a specific position along the spline instead of the closest point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Alpha Mode</strong> <code>EPCGExSplineSampleAlphaMode</code></summary>

How to interpret the alpha value.

| Option       | Description               |
| ------------ | ------------------------- |
| **Alpha**    | Normalized position (0-1) |
| **Distance** | Distance along spline     |

📋 _Visible when Sample Specific Alpha is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Wrap Closed Loops</strong> <code>bool</code></summary>

When enabled, alpha values wrap around for closed splines.

Default: `true`

📋 _Visible when Sample Specific Alpha is enabled_

⚡ PCG Overridable

</details>

#### Weighting

<details>

<summary><strong>Weight Method</strong> <code>EPCGExRangeType</code></summary>

How distance affects weight calculation.

| Option              | Description              |
| ------------------- | ------------------------ |
| **Full Range**      | Use full min-max range   |
| **Effective Range** | Use actual sampled range |

Default: `Full Range`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Over Distance</strong> <code>UCurveFloat</code></summary>

Curve controlling weight falloff based on distance.

⚡ PCG Overridable

</details>

#### Outputs

<details>

<summary><strong>Write Success</strong> <code>bool</code></summary>

Writes sampling success to a boolean attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Transform</strong> <code>bool</code></summary>

Writes the weighted sampled transform from the spline.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Look At</strong> <code>bool</code></summary>

Writes a look-at transform pointing toward the spline.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Distance</strong> <code>bool</code></summary>

Writes the weighted distance to sampled spline.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Signed Distance</strong> <code>bool</code></summary>

Writes signed distance (useful for inside/outside detection).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Alpha</strong> <code>bool</code></summary>

Writes the alpha position (0-1) on the sampled spline.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Tangent</strong> <code>bool</code></summary>

Writes the tangent direction at the sampled point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Samples</strong> <code>bool</code></summary>

Writes the count of sampled splines.

Default: `false`

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Successes</strong> <code>bool</code></summary>

Adds a tag if sampling succeeded for any point.

Default: `false`

</details>

<details>

<summary><strong>Tag If Has No Successes</strong> <code>bool</code></summary>

Adds a tag if sampling failed for all points.

Default: `false`

</details>

### Outputs

| Pin     | Type   | Description                     |
| ------- | ------ | ------------------------------- |
| **Out** | Points | Points with sampled spline data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleNearestSpline.h)
