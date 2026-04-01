---
description: Deform points along a path/spline.
icon: circle
---

# Copy to Path

### Overview

This node takes input points and deforms them along target paths or splines. Points are distributed and transformed to follow the path's shape, with configurable interpolation, scaling, and twist effects.

### How It Works

1. **Match Data**: Paths are matched to point collections based on data matching settings.
2. **Build Splines**: Target paths are converted to splines with configurable point types and tangents.
3. **Calculate Bounds**: Input point bounds are computed to determine the deformation space.
4. **Distribute Points**: Points are mapped along the spline based on their position within the bounds.
5. **Apply Deformation**: Points are transformed to follow the spline, with optional scaling, twist, and masking.

**Usage Notes**

* **Axis Order**: Determines which axis of the input points maps to the spline direction. First axis = along spline.
* **Closed Loops**: When paths are closed loops, points can wrap seamlessly around the loop.
* **Target Masking**: Use mask settings to distribute points over only a portion of each path.

### Behavior

```
Input points:               Deformed along path:

  ● ● ● ● ●                      ●
  ● ● ● ● ●                   ●     ●
  ● ● ● ● ●         →       ●         ●
                          ●             ●
                           ●           ●
                             ● ● ● ● ●

Points follow the path shape while maintaining relative positions.
```

### Inputs

| Pin       | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| **In**    | Points | Points to deform along paths        |
| **Paths** | Points | Target paths to deform points along |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls which paths are used to deform which point collections.

| Property            | Description                                      |
| ------------------- | ------------------------------------------------ |
| **Mode**            | Matching mode (Disabled, One-to-One, etc.).      |
| **Split Unmatched** | Whether to output unmatched points separately.   |
| **Limit Matches**   | Limit the number of paths matched per point set. |

</details>

***

#### Spline Settings

<details>

<summary><strong>Default Point Type</strong> <code>EPCGExSplinePointType</code></summary>

The interpolation type for spline points.

| Option                   | Description                             |
| ------------------------ | --------------------------------------- |
| **Linear**               | Straight lines between points.          |
| **Curve**                | Smooth curves with automatic tangents.  |
| **Constant**             | Step function, no interpolation.        |
| **Curve Clamped**        | Curves with clamped tangents.           |
| **Curve Custom Tangent** | Curves using custom tangent attributes. |

Default: `Curve`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Custom Point Type</strong> <code>bool</code></summary>

Read the spline point type from an attribute per-point.

Default: `false`

Attribute: `PointType`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tangents</strong> <code>FPCGExTangentsDetails</code></summary>

Settings for custom tangent handling when using custom tangent mode.

📋 _Visible when Custom Point Type is enabled or Default Point Type = CurveCustomTangent_

⚡ PCG Overridable

</details>

***

#### Bounds Settings

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which point property to use for calculating the deformation bounds.

| Option             | Description                          |
| ------------------ | ------------------------------------ |
| **Scaled Bounds**  | Use point bounds with scale applied. |
| **Density Bounds** | Use density-based bounds.            |
| **Bounds**         | Use raw point bounds.                |
| **Center**         | Use point centers only.              |

Default: `Center`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Bounds Offset</strong> <code>FVector</code></summary>

Minimum offset for the deformation bounds.

Default: `(-1, -1, -1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Bounds Offset</strong> <code>FVector</code></summary>

Maximum offset for the deformation bounds.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

***

#### Deform Settings

<details>

<summary><strong>Axis Order</strong> <code>EPCGExAxisOrder</code></summary>

Axis transformation order. First axis maps along the spline, second and third are cross-axes.

| Option  | Description                   |
| ------- | ----------------------------- |
| **XYZ** | X along spline, Y and Z cross |
| **YZX** | Y along spline, Z and X cross |
| **ZXY** | Z along spline, X and Y cross |
| ...     | Other permutations            |

Default: `XYZ`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve Original Input Scale</strong> <code>bool</code></summary>

Keep the original scale of input points during deformation.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve Aspect Ratio</strong> <code>bool</code></summary>

Maintain the aspect ratio of points during deformation.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flatten Axis</strong> <code>EPCGExMinimalAxis</code></summary>

Flatten points along a specific axis during deformation.

| Option   | Description     |
| -------- | --------------- |
| **None** | No flattening.  |
| **X**    | Flatten X axis. |
| **Y**    | Flatten Y axis. |
| **Z**    | Flatten Z axis. |

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Wrap Closed Loops</strong> <code>bool</code></summary>

Allow points to wrap seamlessly around closed loop paths.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Main Axis Settings</strong> <code>FPCGExAxisDeformDetails</code></summary>

Controls how points are distributed along the main (spline) axis, including start/end alpha values.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Do Twist</strong> <code>bool</code></summary>

Apply twist rotation along the spline.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Twist Settings</strong> <code>FPCGExAxisDeformDetails</code></summary>

Controls the twist amount at start and end of the path.

Attributes: `StartTwistAmount`, `EndTwistAmount`

📋 _Visible when Do Twist = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target Mask Settings</strong> <code>FPCGExAxisDeformDetails</code></summary>

Shrink the scope per-target to distribute points only on a subselection of the path.

Attributes: `MaskStart`, `MaskEnd`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits point processing settings from its base class.

→ See Points Processor Settings for: Point data handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExCopyToPaths.h)
