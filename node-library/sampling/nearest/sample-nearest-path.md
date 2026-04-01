---
description: Sample the nearest path(s).
icon: circle
---

# Sample : Nearest Path

### Overview

With default settings, this is a **containment sampler** — it answers "which paths contain this point?" rather than "which paths are nearby?" Each source point is tested against paths using 2D projection, and only paths whose closed boundary encloses the point are sampled. Range limits are bypassed for contained points.

Disable **Only Sample When Inside** and **Always Sample When Inside** to switch to standard proximity-based sampling instead.

### How It Works

1. **Gather paths**: Paths within range are collected via octree query. Optionally filtered to only closed loops or open lines.
2. **Find closest point on path**: For each candidate path, the closest point along its edges is computed — or a specific alpha position if enabled.
3. **Test containment**: The source point is tested against the path using 2D projection (point-in-polygon).
4. **Filter**: By default, paths that don't contain the point are skipped. Range limits are bypassed for contained points.
5. **Blend and write**: Qualifying paths are weighted by the distance curve and blended. Results are stored as point attributes.

{% hint style="info" %}
### Containment by default.

Out of the box, this node only samples paths that enclose the source point. Points outside all paths will fail sampling. This is the inverse of [Sample Inside Path](sample-inside-path.md), where paths sample the points they contain.
{% endhint %}

**Usage Notes**

* **2D projection**: Containment testing projects onto a configurable 2D plane. **Height Inclusion** adds an optional vertical tolerance.
* **Sample at specific alpha**: Instead of closest point, sample at a fixed position along the path using alpha (0–1), time, or distance units.
* **Path-specific outputs**: Beyond standard sampler outputs, writes **Time**, **Segment Time**, **Num Inside**, and **Closed Loop**.

### Behavior

```
Source point P:
   Inside Path1 (closed)? ✓  → Sampled (range bypassed)
   Inside Path2 (closed)? ✗  → Skipped
   Near Path3 (open)?     ✗  → Skipped (can't contain)

   → P inherits attributes from Path1

Disable OnlySampleWhenInside:
   → All paths within range sampled by proximity
```

### Inputs

| Pin               | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| **In**            | Points | Source points to sample from               |
| **Paths**         | Points | Target paths to sample                     |
| **Point Filters** | Params | Optional filters for source points         |
| **Sorting Rules** | Params | Optional sorting for Best Candidate method |
| **Blending**      | Params | Optional blending operation factories      |

### Settings

#### Sampling

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how source collections are matched to target paths.

</details>

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

How points and paths are projected for 2D containment testing.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

<details>

<summary><strong>Sample Inputs</strong> <code>EPCGExPathSamplingIncludeMode</code></summary>

Which paths to include in sampling.

| Option                | Description              |
| --------------------- | ------------------------ |
| **All**               | Sample all paths         |
| **Closed Loops Only** | Only sample closed paths |
| **Open Lines Only**   | Only sample open paths   |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Method</strong> <code>EPCGExSampleMethod</code></summary>

How to select which paths to sample.

| Option              | Description                   |
| ------------------- | ----------------------------- |
| **Within Range**    | Sample all paths within range |
| **Closest Target**  | Sample only the closest path  |
| **Farthest Target** | Sample only the farthest path |
| **Best Candidate**  | Sample based on sorting rules |

Default: `Within Range`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Always Sample When Inside</strong> <code>bool</code></summary>

When enabled, always samples paths if the point lies inside, regardless of range.

Default: `true`

</details>

<details>

<summary><strong>Only Sample When Inside</strong> <code>bool</code></summary>

When enabled, only samples closed paths where the point lies inside.

Default: `true`

</details>

<details>

<summary><strong>Inclusion Offset</strong> <code>double</code></summary>

Inset offset applied to paths for containment testing. Positive values shrink the effective containment boundary.

Default: `0`

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

Maximum sampling distance. Can be constant or from attribute. When **Always Sample When Inside** is enabled, this limit is bypassed for points inside paths.

Default: `300`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Height Inclusion</strong> <code>double</code></summary>

If greater than 0, adds a vertical tolerance to containment testing. Points must be within this distance of the path's elevation to count as "inside." A value of 0 means infinite vertical tolerance (pure 2D projection).

Default: `0`

</details>

<details>

<summary><strong>Sample Specific Alpha</strong> <code>bool</code></summary>

When enabled, samples each path at a fixed position instead of finding the closest point on the path edge.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mode</strong> <code>EPCGExPathSampleAlphaMode</code></summary>

How to interpret the sample alpha value.

| Option       | Description                                 |
| ------------ | ------------------------------------------- |
| **Alpha**    | 0–1 normalized position along the path      |
| **Time**     | 0–N value where N is the number of segments |
| **Distance** | Distance along the path in world units      |

Default: `Alpha`

📋 _Visible when Sample Specific Alpha is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Wrap Closed Loops</strong> <code>bool</code></summary>

When enabled, wraps out-of-bounds alpha values on closed loops so they cycle around the path.

Default: `true`

📋 _Visible when Sample Specific Alpha is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Alpha</strong> <code>double</code></summary>

The alpha value to sample at. Can be constant or from attribute.

Default: `0.5`

📋 _Visible when Sample Specific Alpha is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Sampling</strong> <code>FPCGExApplySamplingDetails</code></summary>

Whether to apply sampled transform and look-at directly to source points.

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

Writes the weighted sampled transform.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Look At</strong> <code>bool</code></summary>

Writes a look-at transform pointing toward sampled path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Distance</strong> <code>bool</code></summary>

Writes the weighted distance to sampled path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Signed Distance</strong> <code>bool</code></summary>

Writes signed distance (positive outside, negative inside for closed paths).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Component Wise Distance</strong> <code>bool</code></summary>

Writes distance as a vector with per-axis components.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Angle</strong> <code>bool</code></summary>

Writes the angle between source and sampled path directions.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Time</strong> <code>bool</code></summary>

Writes the normalized position (0–1) along the sampled path where the closest point was found.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Segment Time</strong> <code>bool</code></summary>

Writes the interpolation factor within the matched path segment.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Inside</strong> <code>bool</code></summary>

Writes the count of paths containing this point (via 2D projection containment).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Samples</strong> <code>bool</code></summary>

Writes the count of sampled paths.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Closed Loop</strong> <code>bool</code></summary>

Writes whether the sampled path is a closed loop.

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

#### Advanced

<details>

<summary><strong>Process Filtered Out As Fails</strong> <code>bool</code></summary>

When enabled, filtered-out points are marked as failed samples.

Default: `true`

</details>

<details>

<summary><strong>Prune Failed Samples</strong> <code>bool</code></summary>

When enabled, removes points that failed to sample anything.

Default: `false`

</details>

<details>

<summary><strong>Ignore Self</strong> <code>bool</code></summary>

Excludes a point's own data when source points are also paths.

Default: `true`

</details>

### Outputs

| Pin     | Type   | Description                   |
| ------- | ------ | ----------------------------- |
| **Out** | Points | Points with sampled path data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleNearestPath.h)
