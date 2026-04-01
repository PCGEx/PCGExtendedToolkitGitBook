---
description: Sample nearest target points.
icon: circle
---

# Sample : Nearest Point

### Overview

This node samples data from nearby target points, finding the closest points within range and computing weighted transforms, distances, and blended attributes. It provides extensive output options including look-at transforms, signed distances, angles, and attribute blending from sampled targets.

### How It Works

1. **Find Targets**: Locates target points within the specified range.
2. **Compute Weights**: Calculates weights based on distance or custom attribute.
3. **Blend Data**: Combines sampled attributes using configured blending.
4. **Write Results**: Outputs transforms, distances, and blended values.

{% hint style="info" %}
### Every source point samples independently

This node processes **all input points** — each source point finds and samples its own nearby targets. It does _not_ find "the one closest pair" between inputs and targets.

If you only want specific source points to sample (e.g., only points meeting certain criteria), connect **Point Filters** to control which source points participate in sampling. Filtered-out points pass through unchanged (or can be pruned with "Prune Failed Samples").
{% endhint %}

**Usage Notes**

* **Weight Modes**: Weight by distance, attribute, or constant.
* **Sample Methods**: Sample all, closest, farthest, or best candidate.
* **Attribute Blending**: Transfer and blend target attributes to source points.
* **Apply Directly**: Optionally apply sampled transform directly to points.

### Behavior

**Point Sampling:**

```
Source point P with targets in range (300):
   T1: distance 50, weight attribute = 0.8
   T2: distance 100, weight attribute = 0.2
   T3: distance 200, weight attribute = 1.0

Weight Mode = Distance:
   → T1 gets highest weight (closest)

Weight Mode = Attribute:
   → T3 gets highest weight (attribute = 1.0)

Sample Method = Closest:
   → Only samples T1
```

### Inputs

| Pin               | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| **In**            | Points | Source points to sample from               |
| **Targets**       | Points | Target points to sample                    |
| **Point Filters** | Params | Optional filters for source points         |
| **Sorting Rules** | Params | Optional sorting for Best Candidate method |
| **Blending**      | Params | Optional blending operation factories      |

### Settings

#### Sampling

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how source collections are matched to target points.

</details>

<details>

<summary><strong>Sample Method</strong> <code>EPCGExSampleMethod</code></summary>

How to select which targets to sample.

| Option              | Description                     |
| ------------------- | ------------------------------- |
| **Within Range**    | Sample all targets within range |
| **Closest Target**  | Sample only the closest target  |
| **Farthest Target** | Sample only the farthest target |
| **Best Candidate**  | Sample based on sorting rules   |

Default: `Within Range`

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

#### Weighting

<details>

<summary><strong>Distance Details</strong> <code>FPCGExDistanceDetails</code></summary>

How distance is calculated between points.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Mode</strong> <code>EPCGExSampleWeightMode</code></summary>

What determines sampling weight.

| Option        | Description                  |
| ------------- | ---------------------------- |
| **Distance**  | Weight based on distance     |
| **Attribute** | Weight from target attribute |
| **Constant**  | Equal weight for all targets |

Default: `Distance`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to use for weighting when in Attribute mode.

📋 _Visible when Weight Mode = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Remap</strong> <code>UCurveFloat</code></summary>

Curve that remaps weight values.

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

<summary><strong>Write Angle</strong> <code>bool</code></summary>

Writes the angle between source and target directions.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Num Samples</strong> <code>bool</code></summary>

Writes the count of sampled targets.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Sampled Index</strong> <code>bool</code></summary>

Writes the index of the closest sampled target.

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

Excludes a point's own data when sampling.

Default: `true`

</details>

### Outputs

| Pin     | Type   | Description                     |
| ------- | ------ | ------------------------------- |
| **Out** | Points | Points with sampled target data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleNearestPoint.h)
