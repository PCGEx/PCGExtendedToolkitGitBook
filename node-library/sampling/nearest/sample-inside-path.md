---
description: Sample the points inside the paths.
icon: circle
---

# Sample : Inside Path

### Overview

The inverse of **Sample Nearest Path**. Here, **paths sample data from the target points they contain.** Each path acts as a region — target points inside its 2D projection are collected, blended, and the result is written to the path's `@Data` domain.

### How It Works

1. **Build path boundary**: Each source path is projected to 2D. An optional inclusion offset insets the boundary.
2. **Query target points**: Target points within the path's expanded bounding box are gathered.
3. **Test containment**: Each target is tested against the path via 2D projection. By default, points outside are skipped.
4. **Blend and write**: Qualifying points are blended using distance-based weighting and written to the path's `@Data` domain.

{% hint style="info" %}
**Path-centric, not point-centric.** Paths are the primary input. Results are written per-path to the `@Data` domain, not per-point. This answers "what do the points inside this path look like?" rather than "which path contains this point?"
{% endhint %}

**Usage Notes**

* **Output modes**: Output all paths, only those that sampled at least one point, or split successes and failures to separate pins.
* **Blending**: Connect blending operations to control how multiple target point attributes are combined.
* **Height Inclusion**: Optional vertical tolerance for 3D containment approximation.

### Behavior

```
Source path (closed polygon):
    ┌─────────────────┐
    │  T1    T2       │
    │        ●────────┼──T3 (outside)
    │  ●     ●        │
    └─────────────────┘

With defaults (OnlySampleWhenInside + AlwaysSampleWhenInside):
   T1, T2: inside  → sampled and blended
   T3:     outside → skipped

Path receives blended data → written to @Data domain
```

### Inputs

| Pin               | Type   | Description                                  |
| ----------------- | ------ | -------------------------------------------- |
| **Paths**         | Points | Source paths that define containment regions |
| **Targets**       | Points | Target points to sample from                 |
| **Sorting Rules** | Params | Optional sorting for Best Candidate method   |
| **Blending**      | Params | Optional blending operation factories        |

### Settings

#### Projection

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how point collections are matched to paths for sampling.

</details>

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

How points and paths are projected for 2D containment testing.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

#### Sampling

<details>

<summary><strong>Process Inputs</strong> <code>EPCGExPathSamplingIncludeMode</code></summary>

Which paths to include in sampling.

| Option                | Description              |
| --------------------- | ------------------------ |
| **All**               | Process all paths        |
| **Closed Loops Only** | Only sample closed paths |
| **Open Lines Only**   | Only sample open paths   |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Method</strong> <code>EPCGExSampleMethod</code></summary>

How to select which target points to sample.

| Option              | Description                           |
| ------------------- | ------------------------------------- |
| **Within Range**    | Sample all target points within range |
| **Closest Target**  | Sample only the closest target point  |
| **Farthest Target** | Sample only the farthest target point |
| **Best Candidate**  | Sample based on sorting criteria      |

Default: `Within Range`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Always Sample When Inside</strong> <code>bool</code></summary>

When enabled, always samples target points inside the path even if beyond max edge distance.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Only Sample When Inside</strong> <code>bool</code></summary>

When enabled, only samples target points that lie inside the path boundary.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inclusion Offset</strong> <code>double</code></summary>

Inset offset applied to path bounds for inclusion testing.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Distance Type</strong> <code>EPCGExDistanceType</code></summary>

Method for calculating distances.

| Option        | Description                     |
| ------------- | ------------------------------- |
| **Euclidean** | Standard straight-line distance |
| **Manhattan** | Sum of axis-aligned distances   |
| **Chebyshev** | Maximum of axis distances       |

Default: `Euclidean`

</details>

<details>

<summary><strong>Range Min</strong> <code>double</code></summary>

Minimum distance for sampling. Can be constant or from attribute.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Range Max</strong> <code>double</code></summary>

Maximum distance for sampling. Can be constant or from attribute.

Default: `300`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Height Inclusion</strong> <code>double</code></summary>

Vertical tolerance for 3D containment (0 = infinite). Points outside this height difference are considered outside.

Default: `0`

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

<summary><strong>Output Mode</strong> <code>EPCGExSampleInsidePathOutput</code></summary>

Which paths to output.

| Option           | Description                                       |
| ---------------- | ------------------------------------------------- |
| **All**          | Output all paths                                  |
| **Success Only** | Output only paths that sampled at least one point |
| **Split**        | Split into success and discarded pins             |

Default: `All`

</details>

<details>

<summary><strong>Write Success</strong> <code>bool</code></summary>

When enabled, writes whether the path successfully sampled any target points.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Distance</strong> <code>bool</code></summary>

When enabled, writes the weighted distance to sampled target points.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Inside</strong> <code>bool</code></summary>

When enabled, writes the count of target points inside the path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Samples</strong> <code>bool</code></summary>

When enabled, writes the number of target points sampled by the path.

Default: `false`

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Successes</strong> <code>bool</code></summary>

When enabled, adds a tag to paths that successfully sampled at least one target point.

Default: `false`

</details>

<details>

<summary><strong>Tag If Has No Successes</strong> <code>bool</code></summary>

When enabled, adds a tag to paths that found no target points.

Default: `false`

</details>

#### Advanced

<details>

<summary><strong>Ignore Self</strong> <code>bool</code></summary>

Excludes a path's own data when source paths are also targets.

Default: `true`

</details>

### Outputs

| Pin           | Type   | Description                                       |
| ------------- | ------ | ------------------------------------------------- |
| **Paths**     | Points | Paths with sampled data on `@Data` domain         |
| **Discarded** | Points | Paths that failed sampling (if Split output mode) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleInsidePath.h)
