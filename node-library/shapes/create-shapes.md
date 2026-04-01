---
description: Use shape builders to create shapes from input seed points.
icon: circle
---

# Create Shapes

### Overview

This node generates point collections by applying shape builder definitions to seed points. Each seed point serves as the origin for shape generation, with the shape builder determining the pattern of output points (circles, grids, polygons, etc.). Multiple shape builders can be combined to create layered or composite shapes from each seed.

{% hint style="success" %}
### Scale-Relative Generation

The shape system's key strength is that all shapes compute their geometry relative to their "allocated space" - the seed point's scale/bounds. This means:

* **Adaptive Point Density**: A circle with resolution 10 generates 10 points whether the seed is 1m or 100m in diameter
* **Consistent Topology**: The same shape definition produces consistent point relationships regardless of scale
* **Distance-Based Resolution**: When using distance mode, larger seeds automatically get more points to maintain spacing

This makes shapes ideal for procedural decoration, structural frameworks, and any scenario where point patterns should adapt to varying object sizes.
{% endhint %}

### How It Works

1. **Seed Processing**: Iterates through each input seed point
2. **Shape Generation**: Applies all connected shape builders to generate points around each seed
3. **Validation**: Optionally prunes shapes that fall outside point count thresholds
4. **Output Organization**: Groups results according to the output mode (per dataset, per seed, or per shape)

**Usage Notes**

* **Shape ID Tracking**: Enable Write Shape ID to tag each point with its originating shape index, useful for downstream processing
* **Point Count Pruning**: Use min/max thresholds to filter out degenerate shapes (too few points) or overly complex ones (too many points)
* **Output Modes**: Per Seed mode is useful when you need to process each shape independently; Per Dataset merges everything for bulk operations

### Behavior

```
Seed Points + Shape Builders → Generated Shapes

  Seed •              Circle Builder         Output (Per Seed)
       │                    │
       ▼                    ▼
     [•]  ──────────▶   • • • •           Collection 0: [8 points]
                       •     •
     [•]  ──────────▶  •       •          Collection 1: [8 points]
                       •     •
     [•]  ──────────▶   • • • •           Collection 2: [8 points]

With MinPointCount=2, shapes with <2 points are discarded
```

### Inputs

| Pin                | Type           | Description                                                     |
| ------------------ | -------------- | --------------------------------------------------------------- |
| **In**             | Points         | Seed points that define shape origins                           |
| **Shape Builders** | PCGEx \| Shape | One or more shape builder definitions                           |
| **Point Filters**  | Params         | _Optional._ Filters to select which seed points generate shapes |

### Settings

#### Shape ID

<details>

<summary><strong>Write Shape ID</strong> <code>bool</code></summary>

When enabled, writes an integer attribute to each generated point identifying which shape it belongs to.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Shape ID Attribute Name</strong> <code>FName</code></summary>

The name of the attribute to write the shape ID to.

Default: `ShapeId`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write to Points</strong> <code>bool</code></summary>

Forces writing to point elements rather than the default @Data storage. Only applies when Output Mode is Per Shape.

Default: `false`

📋 _Visible when Output Mode = Per Shape_

</details>

#### Pruning

<details>

<summary><strong>Remove Below</strong> <code>bool</code></summary>

When enabled, discards shapes that have fewer points than the minimum threshold.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Point Count</strong> <code>int32</code></summary>

Shapes with fewer points than this value are discarded.

Default: `2`

Min: `0`

📋 _Visible when Remove Below is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remove Above</strong> <code>bool</code></summary>

When enabled, discards shapes that have more points than the maximum threshold.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Point Count</strong> <code>int32</code></summary>

Shapes with more points than this value are discarded.

Default: `500`

Min: `0`

📋 _Visible when Remove Above is enabled_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits output configuration from its base class.

→ See Shape Processor for: Output Mode

### Outputs

| Pin     | Type   | Description                                                |
| ------- | ------ | ---------------------------------------------------------- |
| **Out** | Points | Generated shape points, organized according to Output Mode |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsShapes-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsShapes/Public/Elements/PCGExCreateShapes.h)
