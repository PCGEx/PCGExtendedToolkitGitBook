---
description: Bevel paths points.
icon: circle
---

# Path : Bevel

### Overview

This node bevels corners along a path, replacing sharp turns with smooth transitions. The bevel can use different profile types (line, arc, or custom) and supports subdivision for smoother curves. Filters control which points get beveled.

### How It Works

1. **Identify Corners**: Points passing the bevel filter are marked for beveling.
2. **Compute Width**: The bevel width is calculated based on mode (radius or distance) and the width setting.
3. **Apply Limits**: Bevels are constrained to avoid overlapping with neighbors or extending past path boundaries.
4. **Generate Profile**: Based on the profile type, new points are created along the bevel curve.
5. **Subdivide**: Optionally add subdivision points along the bevel profile for smoother results.

**Usage Notes**

* **Radius vs Distance**: Radius mode computes the bevel distance from a radius value at the corner angle. Distance mode uses the width directly as segment length.
* **Sliding**: When enabled, bevels can extend past non-beveled points, removing intermediate points as needed.
* **Custom Profile**: Accepts a path input to use as the bevel profile shape, scaled to fit each corner.

### Behavior

```
Before bevel:                After bevel (Line profile):

    ●                            ●
     \                            \
      \                            ●──●
       ●  ← Corner                     \
      /                                 ●
     /                                 /
    ●                                 ●

Corner point is replaced with bevel endpoints.
```

### Inputs

| Pin                  | Type             | Description                                   |
| -------------------- | ---------------- | --------------------------------------------- |
| **In**               | Points           | Path points to bevel                          |
| **Bevel Conditions** | Filter Factories | Filters to determine which points get beveled |
| **Profile**          | Points           | Custom profile path (for Custom type)         |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExBevelMode</code></summary>

How the bevel width is interpreted.

| Option       | Description                                                                         |
| ------------ | ----------------------------------------------------------------------------------- |
| **Radius**   | Width is used as a radius to compute distance along segments based on corner angle. |
| **Distance** | Width is used directly as distance along each neighboring segment.                  |

Default: `Radius`

</details>

<details>

<summary><strong>Type</strong> <code>EPCGExBevelProfileType</code></summary>

The shape of the bevel profile.

| Option     | Description                                 |
| ---------- | ------------------------------------------- |
| **Line**   | Straight line between bevel endpoints.      |
| **Arc**    | Curved arc that smoothly rounds the corner. |
| **Custom** | Use an external path as the profile shape.  |

Default: `Line`

</details>

<details>

<summary><strong>Keep Corner Point</strong> <code>bool</code></summary>

Keep the original corner point in addition to the bevel endpoints. Subdivision is ignored when enabled.

Default: `false`

📋 _Visible when Type = Line_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Width Measure</strong> <code>EPCGExMeanMeasure</code></summary>

How the width value is interpreted.

| Option       | Description                                      |
| ------------ | ------------------------------------------------ |
| **Relative** | Width is relative to segment length (0-1 range). |
| **Discrete** | Width is an absolute distance value.             |

Default: `Relative`

</details>

<details>

<summary><strong>Width Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the bevel width value.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use the constant Width setting.    |
| **Attribute** | Read width from a point attribute. |

Default: `Constant`

</details>

<details>

<summary><strong>Width</strong> <code>double</code></summary>

The bevel width value.

Default: `0.1`

📋 _Visible when Width Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Limit</strong> <code>EPCGExBevelLimit</code></summary>

How bevels are constrained to prevent overlap.

| Option               | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| **None**             | Bevel is not limited (may overlap neighbors).                               |
| **Closest Neighbor** | Limited by the closest neighbor's position.                                 |
| **Balanced**         | Weighted balance against opposing bevels, falling back to closest neighbor. |

Default: `Balanced`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Slide Along Path</strong> <code>bool</code></summary>

Allow bevels to extend past non-beveled points, removing intermediate points. Bevel endpoints traverse along path geometry.

Default: `false`

📋 _Visible when Limit ≠ None_

⚡ PCG Overridable

</details>

***

#### Profile Scaling (Custom Type)

<details>

<summary><strong>Main Axis Scaling</strong> <code>EPCGExBevelCustomProfileScaling</code></summary>

How the custom profile scales along the main (bevel direction) axis.

| Option       | Description                                    |
| ------------ | ---------------------------------------------- |
| **Uniform**  | Keep profile ratio uniform.                    |
| **Scale**    | Use a scale factor relative to bevel distance. |
| **Distance** | Use a fixed distance relative to the corner.   |

Default: `Uniform`

📋 _Visible when Type = Custom_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Cross Axis Scaling</strong> <code>EPCGExBevelCustomProfileScaling</code></summary>

How the custom profile scales along the cross (perpendicular) axis.

Default: `Uniform`

📋 _Visible when Type = Custom_

⚡ PCG Overridable

</details>

***

#### Subdivision Settings

<details>

<summary><strong>Subdivide</strong> <code>bool</code></summary>

Add subdivision points along the bevel profile.

Default: `false`

📋 _Visible when Type ≠ Custom_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Subdivide Method</strong> <code>EPCGExSubdivideMode</code></summary>

How subdivision points are distributed.

| Option        | Description                                        |
| ------------- | -------------------------------------------------- |
| **Distance**  | Points spaced at fixed distance intervals.         |
| **Count**     | Fixed number of subdivision points.                |
| **Manhattan** | Grid-aligned subdivision using Manhattan distance. |

Default: `Count`

📋 _Visible when Subdivide = true and Type ≠ Custom_

⚡ PCG Overridable

</details>

***

#### Flag Settings

<details>

<summary><strong>Flag Poles</strong> <code>bool</code></summary>

Write a boolean attribute marking bevel pole points (either start or end).

Default: `false`

Attribute Name: `IsBevelPole`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flag Start Point</strong> <code>bool</code></summary>

Write a boolean attribute marking bevel start points.

Default: `false`

Attribute Name: `IsBevelStart`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flag End Point</strong> <code>bool</code></summary>

Write a boolean attribute marking bevel end points.

Default: `false`

Attribute Name: `IsBevelEnd`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flag Subdivision</strong> <code>bool</code></summary>

Write a boolean attribute marking subdivision points.

Default: `false`

Attribute Name: `IsSubdivision`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExBevelPath.h)
