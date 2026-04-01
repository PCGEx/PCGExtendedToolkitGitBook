---
description: Sample nearest target bounds.
icon: circle
---

# Sample : Nearest Bounds

### Overview

Tests which target oriented bounding boxes (OBBs) contain each source point, then samples data from the matching targets. Despite the name, this is a **containment test**, not a proximity search — a source point only samples targets whose bounds it falls inside.

### How It Works

1. **Build target OBBs**: Each target point's bounds are converted to an OBB based on the Bounds Source setting and indexed in an octree.
2. **Test containment**: For each source point, query target OBBs and keep only those that contain the point.
3. **Select and blend**: Among containing OBBs, the sample method picks which to use. Results are weighted by the remap curve and blended.
4. **Write outputs**: Transforms, distances, angles, and blended attributes are stored as point attributes.

{% hint style="info" %}
### Containment, not proximity.

A source point must be **inside** a target's OBB to sample it. Points outside all target bounds will fail sampling. The **Bounds Source** setting controls OBB size and directly affects which points fall inside.
{% endhint %}

**Usage Notes**

* **Sample Methods**: Among containing OBBs, pick all, closest, farthest, largest, smallest, or best candidate.
* **Apply Sampling**: Optionally apply sampled transform and look-at directly to points.

### Behavior

```
Target OBBs:
   T1: center (0,0,0), extents 200
   T3: center (100,0,0), extents 50

Source point P at (50, 0, 0):
   Inside T1? ✓   Inside T3? ✓

   Sample Method = All      → Blends T1 and T3
   Sample Method = Closest  → Samples T3 only
   Sample Method = Largest  → Samples T1 only

Point Q at (500, 0, 0):
   Inside nothing → Sampling fails
```

### Inputs

| Pin               | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| **In**            | Points | Source points to sample from               |
| **Bounds**        | Points | Target bounds to sample                    |
| **Point Filters** | Params | Optional filters for source points         |
| **Sorting Rules** | Params | Optional sorting for Best Candidate method |
| **Blending**      | Params | Optional blending operation factories      |

### Settings

#### Sampling

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how source collections are matched to target bounds for sampling.

</details>

<details>

<summary><strong>Sample Method</strong> <code>EPCGExBoundsSampleMethod</code></summary>

How to select which bounds to sample.

| Option              | Description                                |
| ------------------- | ------------------------------------------ |
| **All**             | Sample all overlapping bounds              |
| **Closest Bounds**  | Sample only the closest bounds             |
| **Farthest Bounds** | Sample only the farthest bounds            |
| **Largest Bounds**  | Sample only the largest bounds (by extent) |
| **Smallest Bounds** | Sample only the smallest bounds            |
| **Best Candidate**  | Sample based on sorting rules              |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use from target points.

| Option             | Description                |
| ------------------ | -------------------------- |
| **Scaled Bounds**  | Bounds multiplied by scale |
| **Density Bounds** | Density-based bounds       |
| **Bounds**         | Raw point bounds           |
| **Center**         | Point center only          |

Default: `Scaled Bounds`

</details>

<details>

<summary><strong>Distance Type</strong> <code>EPCGExDistanceType</code></summary>

Method for calculating distances.

Default: `Euclidean`

</details>

<details>

<summary><strong>Apply Sampling</strong> <code>FPCGExApplySamplingDetails</code></summary>

Whether to apply sampled transform and look-at directly to source points.

</details>

#### Weighting

<details>

<summary><strong>Weight Remap</strong> <code>UCurveFloat</code></summary>

Curve that remaps distance to weight for blending.

⚡ PCG Overridable

</details>

#### Blending

<details>

<summary><strong>Blending Interface</strong> <code>EPCGExBlendingInterface</code></summary>

How blending operations are configured.

| Option         | Description                      |
| -------------- | -------------------------------- |
| **Individual** | Use connected Blending sub-nodes |
| **Monolithic** | Configure all blending in-node   |

Default: `Individual`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target Attributes</strong> <code>TMap</code></summary>

Map of attribute names to blending types for monolithic mode.

📋 _Visible when Blending Interface = Monolithic_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Point Properties</strong> <code>bool</code></summary>

When enabled, blends point properties (density, color, etc.).

Default: `false`

📋 _Visible when Blending Interface = Monolithic_

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

Writes a look-at transform pointing toward sampled targets.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Distance</strong> <code>bool</code></summary>

Writes the weighted distance to sampled targets.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Signed Distance</strong> <code>bool</code></summary>

Writes signed distance along a specified axis.

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

Writes the angle between source and target directions.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Samples</strong> <code>bool</code></summary>

Writes the count of sampled bounds.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Sampled Index</strong> <code>bool</code></summary>

Writes the index of the closest sampled bounds.

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

Excludes a point's own data when sampling bounds.

Default: `true`

</details>

### Outputs

| Pin     | Type   | Description              |
| ------- | ------ | ------------------------ |
| **Out** | Points | Points with sampled data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleNearestBounds.h)
