---
description: Base settings shared by all neighbor sampling sub-nodes.
icon: arrow-down-from-arc
---

# Neighbor Sampler

### Overview

This is the abstract base class for all neighbor sampler factories. It defines the common sampling configuration used across different sampler types, including traversal depth, weight blending options, and weight curves. Individual sampler sub-nodes inherit these settings and add their specific attribute or property sampling logic.

### How It Works

1. **Configure Traversal**: Set maximum depth for neighbor exploration
2. **Set Weight Blending**: Choose how sample weights are computed (by index, distance, or fixed)
3. **Apply Weight Curve**: Optionally remap weights through a curve for non-linear falloff
4. **Select Source**: Choose whether to sample from vertices or edges
5. **Execute Sampling**: Derived samplers use this configuration during neighbor traversal

### Behavior

```
Sampling Configuration:

MaxDepth = 2:
  ●───●───●───●───●
  A   B   C   D   E

  From A: samples B (depth 1), C (depth 2)
  D and E are beyond max depth

BlendOver Options:
  Index:    Weight based on neighbor order (1st, 2nd, 3rd...)
  Distance: Weight based on distance from source
  Fixed:    All neighbors use same weight

Weight Curve Effect:
  Linear:   ●───●───●───●  (equal falloff)
  EaseOut:  ●──●─●──●      (fast start, slow end)
  EaseIn:   ●──────●─●──●  (slow start, fast end)
```

### Settings

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Processing priority for this sampler. Higher values are processed last, allowing samplers to build on results from earlier ones.

Default: `0`

⚡ PCG Overridable

</details>

#### Sampling Configuration

<details>

<summary><strong>Range Type</strong> <code>EPCGExRangeType</code></summary>

Determines the range used for weight blending computation.

| Option              | Description                           |
| ------------------- | ------------------------------------- |
| **Full Range**      | Use the full range of neighbors       |
| **Effective Range** | Use only the effective neighbor range |

Default: `Full Range`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Depth</strong> <code>int32</code></summary>

Maximum traversal depth for neighbor sampling. A depth of 1 samples only immediate neighbors, depth 2 includes neighbors of neighbors, and so on.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Over</strong> <code>EPCGExBlendOver</code></summary>

How to compute the initial blend weight for each neighbor sample.

| Option       | Description                                 |
| ------------ | ------------------------------------------- |
| **Index**    | Weight based on neighbor index order        |
| **Distance** | Weight based on distance from source vertex |
| **Fixed**    | All neighbors use the same fixed weight     |

Default: `Index`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Distance</strong> <code>double</code></summary>

Maximum distance for weight falloff when using distance-based blending. Neighbors at this distance have zero weight.

Default: `300`

📋 _Visible when Blend Over = Distance_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fixed Blend</strong> <code>double</code></summary>

The fixed weight value applied to all neighbor samples.

Default: `1`

📋 _Visible when Blend Over = Fixed_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Local Curve</strong> <code>bool</code></summary>

When enabled, uses an in-editor curve instead of an external curve asset for weight remapping.

Default: `false`

</details>

<details>

<summary><strong>Weight Curve (Local)</strong> <code>FRuntimeFloatCurve</code></summary>

In-editor curve for remapping sample weights. The X axis represents normalized distance/index (0-1), Y axis is the output weight multiplier.

📋 _Visible when Use Local Curve = true_

</details>

<details>

<summary><strong>Weight Curve</strong> <code>TSoftObjectPtr&#x3C;UCurveFloat></code></summary>

External curve asset for remapping sample weights. Allows sharing weight curves across multiple nodes.

Default: `WeightDistributionLinear`

📋 _Visible when Use Local Curve = false_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Curve Lookup</strong> <code>FPCGExCurveLookupDetails</code></summary>

Configuration for curve lookup table generation, affecting curve sampling precision and performance.

</details>

<details>

<summary><strong>Neighbor Source</strong> <code>EPCGExClusterElement</code></summary>

Which cluster element to sample from.

| Option   | Description             |
| -------- | ----------------------- |
| **Vtx**  | Sample from vertex data |
| **Edge** | Sample from edge data   |

Default: `Vtx`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/NeighborSamplers/PCGExNeighborSampleFactoryProvider.h)
