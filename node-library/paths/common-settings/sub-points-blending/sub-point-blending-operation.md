---
description: Base factory for sub-point attribute blending operations.
icon: arrow-down-from-arc
---

# Sub-Point Blending Operation

### Overview

This abstract factory provides the common blending configuration used by all sub-point blending methods. It defines which attributes should be blended and how they should be interpolated between neighboring points when sub-points are inserted along paths.

### How It Works

1. **Filter Selection**: Determines which attributes participate in blending using inclusion/exclusion rules
2. **Blending Strategy**: Applies the specified blending type (lerp, average, min, max, etc.) to each attribute
3. **Override Management**: Allows specific attributes or properties to use different blending methods than the default

**Usage Notes**

* **Base Class**: This is not used directly - specific blending implementations inherit these settings
* **Shared Configuration**: All blending methods (Interpolate, Inherit Start, Inherit End) use these same settings to control which attributes are affected

### Settings

#### Blending Configuration

<details>

<summary><strong>Blending Filter</strong> <code>EPCGExAttributeFilter</code></summary>

Controls which attributes participate in the blending operation.

| Option      | Description                                                       |
| ----------- | ----------------------------------------------------------------- |
| **All**     | Blend all attributes                                              |
| **Include** | Blend only attributes in the Filtered Attributes list             |
| **Exclude** | Blend all attributes except those in the Filtered Attributes list |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Filtered Attributes</strong> <code>TArray</code></summary>

List of attribute names to include or exclude from blending, depending on the Blending Filter mode.

Default: Empty

⚡ PCG Overridable

📋 _Visible when Blending Filter = Include or Exclude_

</details>

<details>

<summary><strong>Default Blending</strong> <code>EPCGExBlendingType</code></summary>

The blending method applied to attributes that don't have a specific override.

| Option      | Description                                       |
| ----------- | ------------------------------------------------- |
| **None**    | No blending (keeps original value)                |
| **Average** | Arithmetic mean of neighboring values             |
| **Lerp**    | Linear interpolation based on position along path |
| **Min**     | Minimum of neighboring values                     |
| **Max**     | Maximum of neighboring values                     |
| **Copy**    | Copy from source                                  |

Default: `Average`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Properties Overrides</strong> <code>FPCGExPointPropertyBlendingOverrides</code></summary>

Specifies custom blending methods for standard point properties (Position, Rotation, Scale, Color, etc.) that differ from the default blending type.

Default: None

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attributes Overrides</strong> <code>TMap</code></summary>

Map of attribute names to specific blending types, allowing individual attributes to use different blending methods than the default.

Default: Empty

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/SubPoints/DataBlending/PCGExSubPointsBlendOperation.h)
