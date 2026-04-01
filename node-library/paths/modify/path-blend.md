---
description: Blend path individual points between its start and end points.
icon: circle
---

# Path : Blend

### Overview

This node blends the properties and attributes of points along a path, interpolating between the first and last point values. The blend factor can be based on distance along the path, point index, or a fixed value.

### How It Works

1. **Identify Endpoints**: The first and last points of the path are used as blend sources.
2. **Calculate Blend Factor**: For each interior point, compute a blend factor (0-1) based on the selected mode.
3. **Apply Blending**: Interpolate attributes from the start point to the end point using the blend factor and configured blend operations.

**Usage Notes**

* **Distance Mode**: Blend factor is based on actual path distance, accounting for varying segment lengths.
* **Index Mode**: Blend factor is based on point index (uniform distribution regardless of spacing).
* **Fixed Mode**: All points use the same blend factor, useful for uniform shifts toward one endpoint.

### Behavior

```
Blend Over Distance:

Start ●────●────●────────────●────● End
      0   0.2  0.4          0.8   1.0
           ↓
      Blend factor based on cumulative distance

Attributes interpolate smoothly from start values to end values.
```

### Inputs

| Pin          | Type               | Description                        |
| ------------ | ------------------ | ---------------------------------- |
| **In**       | Points             | Path points to blend               |
| **Blending** | Blend Op Factories | Optional blend operations to apply |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Blend Over</strong> <code>EPCGExBlendOver</code></summary>

How the blend factor is calculated for each point.

| Option       | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| **Distance** | Blend factor based on cumulative path distance (0 at start, 1 at end). |
| **Count**    | Blend factor based on point index (uniform distribution).              |
| **Fixed**    | All points use the same fixed blend factor.                            |

Default: `Distance`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lerp Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the fixed lerp value.

| Option        | Description                             |
| ------------- | --------------------------------------- |
| **Constant**  | Use the constant Lerp setting.          |
| **Attribute** | Read lerp value from a point attribute. |

Default: `Constant`

📋 _Visible when Blend Over = Fixed_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lerp</strong> <code>double</code></summary>

Fixed blend factor applied to all points. 0 = start values, 1 = end values.

Default: `0.5`

📋 _Visible when Blend Over = Fixed and Lerp Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blending Settings</strong> <code>FPCGExBlendingDetails</code></summary>

Controls how attributes are blended between start and end points.

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Blend First Point</strong> <code>bool</code></summary>

Apply blending to the first point. Useful with certain blend modes that modify based on neighbors.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Last Point</strong> <code>bool</code></summary>

Apply blending to the last point. Useful with certain blend modes that modify based on neighbors.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExBlendPath.h)
