---
description: Create points in a 3D grid shape.
icon: circle-dashed
---

# Shape : 3D Grid

### Overview

This shape builder generates points arranged in a three-dimensional grid pattern. The grid dimensions are derived from the seed point's scale, with resolution controlling the point density along each axis. Per-axis truncation and clamping options provide fine control over the final grid dimensions when using distance-based resolution.

### How It Works

1. **Calculate Dimensions**: Gets grid extents from seed point scale.
2. **Determine Cell Count**: Computes points per axis based on resolution.
3. **Apply Clamping**: Limits point counts per axis if configured.
4. **Generate Grid**: Creates points at regular intervals in 3D space.

**Usage Notes**

* **3D by Default**: Creates volumetric grids, not just 2D planes.
* **Per-Axis Control**: Each axis can have independent clamping.
* **Fit Adjustment**: Can adjust extents to fit cells exactly.
* **Resolution Modes**: Fixed count or distance-based spacing.

### Behavior

**Grid Generation:**

```
Seed point with Scale = (100, 100, 50):
   Extents: 100 x 100 x 50

Resolution Mode = Fixed, Resolution = 5:
   → 5 x 5 x 5 = 125 points (uniform count per axis)

Resolution Mode = Distance, Resolution = 20:
   X: 100/20 = 5 cells
   Y: 100/20 = 5 cells
   Z: 50/20 = 2.5 → truncated to 2 or 3 cells
```

### Outputs

| Pin       | Type  | Description                   |
| --------- | ----- | ----------------------------- |
| **Shape** | Shape | 3D grid shape builder factory |

### Settings

#### Resolution Per Axis

<details>

<summary><strong>Adjust Fit</strong> <code>bitmask</code></summary>

Which axes should have their extents adjusted to fit cells exactly. Uses component flags (X, Y, Z).

Default: `XYZ` (all axes)

</details>

<details>

<summary><strong>X - Round</strong> <code>EPCGExTruncateMode</code></summary>

How to round the X-axis cell count when using distance-based resolution.

| Option    | Description      |
| --------- | ---------------- |
| **None**  | No rounding      |
| **Round** | Round to nearest |
| **Floor** | Round down       |
| **Ceil**  | Round up         |

Default: `None`

📋 _Visible when Resolution Mode = Distance_

</details>

<details>

<summary><strong>X - Clamp Count</strong> <code>FPCGExClampDetails</code></summary>

Clamps the X-axis point count to a min/max range.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Y - Round</strong> <code>EPCGExTruncateMode</code></summary>

How to round the Y-axis cell count when using distance-based resolution.

Default: `None`

📋 _Visible when Resolution Mode = Distance_

</details>

<details>

<summary><strong>Y - Clamp Count</strong> <code>FPCGExClampDetails</code></summary>

Clamps the Y-axis point count to a min/max range.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Z - Round</strong> <code>EPCGExTruncateMode</code></summary>

How to round the Z-axis cell count when using distance-based resolution.

Default: `None`

📋 _Visible when Resolution Mode = Distance_

</details>

<details>

<summary><strong>Z - Clamp Count</strong> <code>FPCGExClampDetails</code></summary>

Clamps the Z-axis point count to a min/max range.

⚡ PCG Overridable

</details>

#### Shape Settings (Inherited)

<details>

<summary><strong>Three Dimensions</strong> <code>bool</code></summary>

When enabled, generates a 3D grid. When disabled, generates a 2D grid.

Default: `true` (3D grid enabled by default)

</details>

<details>

<summary><strong>Resolution Mode</strong> <code>EPCGExResolutionMode</code></summary>

How resolution is interpreted.

| Option       | Description                     |
| ------------ | ------------------------------- |
| **Fixed**    | Exact number of points per axis |
| **Distance** | Target distance between points  |

Default: `Fixed`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Resolution</strong> <code>double</code></summary>

Number of points per axis (Fixed mode) or distance between points (Distance mode).

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fitting</strong> <code>FPCGExFittingDetailsHandler</code></summary>

Controls how the grid fits within the seed point's bounds.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Points Look At</strong> <code>EPCGExShapePointLookAt</code></summary>

Orientation mode for generated points.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Extents</strong> <code>FVector</code></summary>

Default grid size when seed has no scale information.

Default: `(50, 50, 50)`

⚡ PCG Overridable

</details>

#### Pruning (Inherited)

<details>

<summary><strong>Remove Below</strong> <code>bool</code></summary>

Discard shapes with fewer points than minimum.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Point Count</strong> <code>int32</code></summary>

Minimum points required.

Default: `2`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remove Above</strong> <code>bool</code></summary>

Discard shapes with more points than maximum.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Point Count</strong> <code>int32</code></summary>

Maximum points allowed.

Default: `500`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsShapes-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsShapes/Public/Shapes/PCGExShapeGrid.h)
