---
description: Blend attributes between source and destination points.
icon: function
---

# Interpolate

### Overview

This blending option interpolates attribute values between the source and destination points based on each sub-point's position along the path segment. The interpolation factor can be determined by distance, index count, or a fixed value, allowing for smooth attribute transitions across inserted sub-points.

> This is a [sub-point-blending-operation.md](sub-point-blending-operation.md "mention")

### How It Works

1. **Calculate Position**: Determines each sub-point's relative position along the segment
2. **Compute Blend Factor**: Calculates the interpolation weight based on the selected blend method (distance, index, or fixed)
3. **Interpolate Attributes**: Blends attribute values between source and destination using the calculated factor

**Usage Notes**

* **Smooth Gradients**: Creates smooth attribute transitions along the path segment
* **Distance vs Index**: Distance-based blending accounts for actual spatial distribution, while index-based blending treats all sub-points as evenly spaced regardless of position
* **Fixed Mode**: Useful when you want all sub-points to have the same blend ratio regardless of their position

### Behavior

```
Distance Mode:
Input Points:     A[value=0] ──────────── B[value=100]
                             │  │    │      │
Sub-points at:             25% 40%  60%   90% distance

Result:                   S1[25]
                              S2[40]
                                   S3[60]
                                          S4[90]

Index Mode:
4 sub-points evenly interpolated:
                          S1[20]
                              S2[40]
                                   S3[60]
                                          S4[80]

Fixed Mode (Lerp=0.3):
All sub-points get 30% blend:
                          S1[30]
                              S2[30]
                                   S3[30]
                                          S4[30]
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Blend Over</strong> <code>EPCGExBlendOver</code></summary>

Determines how the interpolation factor is calculated for each sub-point.

| Option       | Description                                                                         |
| ------------ | ----------------------------------------------------------------------------------- |
| **Distance** | Interpolate based on distance along the segment (accounts for spatial distribution) |
| **Index**    | Interpolate based on sub-point index count (treats all sub-points as evenly spaced) |
| **Fixed**    | Use a constant interpolation value for all sub-points                               |

Default: `Distance`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lerp</strong> <code>double</code></summary>

The fixed interpolation factor to use when Blend Over is set to Fixed. A value of 0.0 means fully inherit from the source point, 1.0 means fully inherit from the destination point, and values in between create a blend of both.

Default: `0.5`

⚡ PCG Overridable

📋 _Visible when Blend Over = Fixed_

</details>

#### Inherited Settings

This factory inherits blending configuration from its base class.

→ See Sub-Point Blending Base for: Blending Filter, Filtered Attributes, Default Blending, Properties Overrides, Attributes Overrides

The Default Blending for this factory is set to `Lerp` (linear interpolation) by default.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/SubPoints/DataBlending/PCGExSubPointsBlendInterpolate.h)
