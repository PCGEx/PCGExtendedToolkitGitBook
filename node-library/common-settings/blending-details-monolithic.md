---
description: >-
  Configures how point properties and attributes are combined when merging or
  fusing points.
icon: sliders-simple
---

# Blending Details (Monolithic)

### Overview

This settings block controls attribute blending — the process of combining values from multiple source points into a single result. When points are fused, merged, or interpolated, these settings determine how each attribute is combined: averaged, summed, copied from one source, or processed using other mathematical operations. You can set a default blend mode and override it for specific properties or attributes.

### How It Works

1. **Filter Attributes**: Optionally include or exclude specific attributes from blending
2. **Apply Default Mode**: Unspecified attributes use the default blending type
3. **Apply Overrides**: Properties and named attributes can use custom blend modes
4. **Compute Results**: Each attribute value is calculated according to its blend mode

### Behavior

```
Blending Example (3 points merged):

Point A: Density=1.0, Color=(1,0,0)
Point B: Density=2.0, Color=(0,1,0)
Point C: Density=3.0, Color=(0,0,1)

Default Blending: Average
┌─────────────────────────────────────────────────┐
│ Density: (1.0 + 2.0 + 3.0) / 3 = 2.0            │
│ Color:   Average RGB = (0.33, 0.33, 0.33)       │
└─────────────────────────────────────────────────┘

With Override: Density = Max
┌─────────────────────────────────────────────────┐
│ Density: Max(1.0, 2.0, 3.0) = 3.0               │
│ Color:   Still averaged                         │
└─────────────────────────────────────────────────┘
```

### Settings

<details>

<summary><strong>Blending Filter</strong> <code>EPCGExAttributeFilter</code></summary>

Controls which attributes participate in blending.

| Option      | Description                                            |
| ----------- | ------------------------------------------------------ |
| **All**     | All attributes are blended                             |
| **Exclude** | Listed attributes are skipped; others are blended      |
| **Include** | Only listed attributes are blended; others are skipped |

Default: `All`

</details>

<details>

<summary><strong>Filtered Attributes</strong> <code>TArray&#x3C;FName></code></summary>

List of attribute names to include or exclude based on the Blending Filter setting.

📋 _Visible when Blending Filter ≠ All_

</details>

<details>

<summary><strong>Default Blending</strong> <code>EPCGExBlendingType</code></summary>

The blend mode applied to attributes without specific overrides.

| Option           | Description                           |
| ---------------- | ------------------------------------- |
| **None**         | No blending; values are unchanged     |
| **Average**      | Arithmetic mean of all values         |
| **Weight**       | Weighted average using point weights  |
| **Min**          | Smallest value                        |
| **Max**          | Largest value                         |
| **Copy**         | Copy from first/primary point         |
| **Sum**          | Add all values together               |
| **Weighted Sum** | Sum multiplied by weights             |
| **Lerp**         | Linear interpolation based on alpha   |
| **Subtract**     | Subtract subsequent values from first |

Default: `Average`

</details>

<details>

<summary><strong>Properties Overrides</strong> <code>FPCGExPointPropertyBlendingOverrides</code></summary>

Override blend modes for built-in point properties (Density, Position, Rotation, Scale, Color, Bounds, Steepness, Seed). Each property can be individually configured with its own blend mode.

</details>

<details>

<summary><strong>Attributes Overrides</strong> <code>TMap&#x3C;FName, EPCGExBlendingType></code></summary>

Override blend modes for specific named attributes. Map attribute names to their desired blend mode.

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Details/PCGExBlendingDetails.h)
