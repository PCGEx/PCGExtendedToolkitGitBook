---
description: Create points in a circular shape.
icon: circle-dashed
---

# Shape : Circle

### Overview

This shape builder generates points arranged in a circular pattern around a center point. It supports full circles, arcs with configurable start and end angles, and can create open or closed loops. The radius is derived from the seed point's scale, and the number of points is controlled by resolution settings.

### How It Works

1. **Read Configuration**: Gets radius from seed scale, angles from settings.
2. **Calculate Points**: Distributes points evenly along the arc.
3. **Apply Transform**: Positions points relative to seed location and rotation.
4. **Output Shape**: Returns point collection for the circle/arc.

**Usage Notes**

* **Radius**: Determined by the seed point's scale (extents).
* **Arc Support**: Use Start/End Angle to create partial arcs.
* **Closed Loop**: Full 360° circles can be flagged as closed for path operations.
* **Resolution**: Controls point density along the circumference.

### Behavior

**Circle Generation:**

```
Seed point with Scale = (100, 100, 100):
   Radius = 100 (from scale)

Full Circle (0° to 360°):
   Resolution = 8
   → 8 points evenly spaced around circle

Arc (0° to 180°):
   Resolution = 8
   → 5 points along semicircle (half resolution + 1)
```

### Outputs

| Pin       | Type  | Description                  |
| --------- | ----- | ---------------------------- |
| **Shape** | Shape | Circle shape builder factory |

### Settings

#### Circle Configuration

<details>

<summary><strong>Start Angle Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the start angle value.

| Option        | Description               |
| ------------- | ------------------------- |
| **Constant**  | Use fixed angle value     |
| **Attribute** | Read from point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Start Angle</strong> <code>double</code></summary>

Starting angle for the circle/arc in degrees.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>End Angle Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the end angle value.

| Option        | Description               |
| ------------- | ------------------------- |
| **Constant**  | Use fixed angle value     |
| **Attribute** | Read from point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>End Angle</strong> <code>double</code></summary>

Ending angle for the circle/arc in degrees.

Default: `360`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Is Closed Loop</strong> <code>bool</code></summary>

When enabled and the arc forms a complete circle, flags the output as a closed loop. Useful for path operations that need to know if the shape connects back to its start.

Default: `true`

⚡ PCG Overridable

</details>

#### Shape Settings (Inherited)

<details>

<summary><strong>Three Dimensions</strong> <code>bool</code></summary>

When enabled, generates a 3D shape instead of 2D.

Default: `false`

</details>

<details>

<summary><strong>Resolution Mode</strong> <code>EPCGExResolutionMode</code></summary>

How resolution is interpreted.

| Option       | Description              |
| ------------ | ------------------------ |
| **Fixed**    | Exact number of points   |
| **Distance** | Points per unit distance |

Default: `Fixed`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Resolution</strong> <code>double</code></summary>

Number of points or distance between points depending on mode.

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fitting</strong> <code>FPCGExFittingDetailsHandler</code></summary>

Controls how the shape fits within the seed point's bounds.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Points Look At</strong> <code>EPCGExShapePointLookAt</code></summary>

Orientation mode for generated points.

| Option       | Description                    |
| ------------ | ------------------------------ |
| **None**     | No special orientation         |
| **Center**   | Points face the center         |
| **Outward**  | Points face away from center   |
| **Next**     | Points face the next point     |
| **Previous** | Points face the previous point |

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Extents</strong> <code>FVector</code></summary>

Default size when seed has no scale information.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsShapes-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsShapes/Public/Shapes/PCGExShapeCircle.h)
