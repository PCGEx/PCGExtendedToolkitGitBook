---
description: Solidify a path.
icon: circle
---

# Path : Solidify

### Overview

This node converts discrete path points into segment-like representations by modifying each point's bounds to span from its position toward the next point. The primary axis bounds extend along the path edge length, while secondary and tertiary axes define the cross-sectional dimensions using configurable radii.

### How It Works

1. **Compute Edge Direction**: For each point, calculate the direction and length to the next point.
2. **Compute Normal**: Determine the perpendicular direction based on the selected normal method.
3. **Reorder Axes**: Apply the solidification order to map local axes to path geometry.
4. **Set Bounds**: Extend the primary axis bounds along the edge length; set secondary/tertiary bounds based on radius values.
5. **Orient Point**: Rotate the point to align with the computed coordinate frame.

**Usage Notes**

* **Bounds Representation**: Each point's bounds become a segment slice - the primary axis spans the edge length while secondary/tertiary axes define the cross-section.
* **Axis Order**: The order determines which local axis (X, Y, Z) aligns with the edge direction. Choose based on how downstream nodes interpret bounds.
* **Radii**: Secondary and tertiary radii define the cross-sectional dimensions of the solidified segment (its "thickness").
* **Last Point**: For open paths, the last point has no following edge, so it's typically removed.

### Behavior

```
Solidify converts point bounds to segment representations:

Before (discrete points with default bounds):
●─────────●─────────●─────────●
(each point has its own small bounds)

After (bounds span edge length with cross-section radii):
[■■■■■■■■■][■■■■■■■■■][■■■■■■■■■]
 ← edge →  ← edge →  ← edge →

Each point's bounds now represent:
- Primary axis: spans from this point toward the next
- Secondary/Tertiary: cross-sectional dimensions (radii)

Top-down view of a solidified point:
        ┌─────────────────┐
        │    Secondary    │
        │   ←── radius ──→│
        │                 │ Tertiary
        ●─────────────────● radius
        │   (edge length) │  ↕
        │                 │
        └─────────────────┘
```

### Inputs

| Pin    | Type   | Description             |
| ------ | ------ | ----------------------- |
| **In** | Points | Path points to solidify |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Remove Last Point</strong> <code>bool</code></summary>

Remove the last point of open paths. The last point has no following edge to solidify into.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Solidification Order</strong> <code>EPCGExAxisOrder</code></summary>

Which local axis aligns with which computed direction. Primary = edge direction, Secondary = normal, Tertiary = cross product.

| Option        | Description                                       |
| ------------- | ------------------------------------------------- |
| **X > Y > Z** | X = edge direction, Y = normal, Z = perpendicular |
| **Y > Z > X** | Y = edge direction, Z = normal, X = perpendicular |
| **Z > X > Y** | Z = edge direction, X = normal, Y = perpendicular |
| **Y > X > Z** | Y = edge direction, X = normal, Z = perpendicular |
| **Z > Y > X** | Z = edge direction, Y = normal, X = perpendicular |
| **X > Z > Y** | X = edge direction, Z = normal, Y = perpendicular |

Default: `X > Y > Z`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation Construction</strong> <code>EPCGExMakeRotAxis</code></summary>

Defines how the rotation is built from the computed directions.

Default: `X`

📋 _Visible when Use Construction Mapping = false_

⚡ PCG Overridable

</details>

***

#### Axis Settings

<details>

<summary><strong>Primary</strong> <code>FPCGExPathSolidificationAxisDetails</code></summary>

Settings for the primary axis (aligned with edge direction). Bounds along this axis span the edge length.

| Property       | Description                                             |
| -------------- | ------------------------------------------------------- |
| **Flip Input** | How flip is determined (Disabled, Constant, Attribute). |
| **Flip**       | Invert the axis direction.                              |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary</strong> <code>FPCGExPathSolidificationRadiusDetails</code></summary>

Settings for the secondary axis. Bounds are set symmetrically using the radius value.

| Property         | Description                                               |
| ---------------- | --------------------------------------------------------- |
| **Flip Input**   | How flip is determined.                                   |
| **Flip**         | Invert the axis direction.                                |
| **Radius Input** | How radius is determined (Disabled, Constant, Attribute). |
| **Radius**       | Half-extent of bounds on this axis. Default: `10`         |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tertiary</strong> <code>FPCGExPathSolidificationRadiusDetails</code></summary>

Settings for the tertiary axis. Bounds are set symmetrically using the radius value.

| Property         | Description                                               |
| ---------------- | --------------------------------------------------------- |
| **Flip Input**   | How flip is determined.                                   |
| **Flip**         | Invert the axis direction.                                |
| **Radius Input** | How radius is determined (Disabled, Constant, Attribute). |
| **Radius**       | Half-extent of bounds on this axis. Default: `10`         |

⚡ PCG Overridable

</details>

***

#### Normal Settings

<details>

<summary><strong>Normal Type</strong> <code>EPCGExInputValueType</code></summary>

Source for the perpendicular (normal) direction used to orient the cross-section.

| Option        | Description                                   |
| ------------- | --------------------------------------------- |
| **Constant**  | Use the computed path normal type below.      |
| **Attribute** | Read normal direction from a point attribute. |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normal</strong> <code>EPCGExPathNormalDirection</code></summary>

Which path-derived direction to use as the normal.

| Option             | Description                     |
| ------------------ | ------------------------------- |
| **Normal**         | The path's normal vector.       |
| **Binormal**       | The path's binormal vector.     |
| **Average Normal** | Average of neighboring normals. |

Default: `Normal`

📋 _Visible when Normal Type = Constant_

⚡ PCG Overridable

</details>

***

#### Blending

<details>

<summary><strong>Solidification Lerp Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the lerp value controlling position along the edge.

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Solidification Lerp</strong> <code>double</code></summary>

Controls where along the edge the point is positioned (0 = at original position, 1 = at next point). Also affects how the bounds are distributed along the edge.

Default: `0`

📋 _Visible when Solidification Lerp Input = Constant_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathSolidify.h)
