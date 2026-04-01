---
description: Offset path points.
icon: circle
---

# Path : Offset

{% hint style="success" %}
### Superseded by [clipper2-offset.md](../generate/clipper2-offset.md "mention")

While this offset node has still many valid uses, [clipper2-offset.md](../generate/clipper2-offset.md "mention") is a much better choice if you want robust self-intersection handling.
{% endhint %}

### Overview

This node displaces path points perpendicular to the path direction, creating parallel paths or expanding/contracting path shapes. The offset direction is computed from the path geometry using configurable methods, with options for handling tight corners.

### How It Works

1. **Compute Direction**: For each point, calculate the offset direction based on the path geometry and the selected method.
2. **Apply Offset**: Displace each point along its offset direction by the specified amount.
3. **Adjust Corners**: Apply optional adjustment to handle tight angles where offsets would create artifacts.

**Usage Notes**

* **Slide vs Line/Plane**: Slide computes offset from path geometry (normals, binormals). Line/Plane uses geometric intersection for more precise results at corners.
* **Corner Handling**: Tight corners can cause offset paths to self-intersect. Use adjustment options (Smooth, Mitre) to handle these cases.
* **Direction Types**: Average Normal typically produces the smoothest results. Normal and Binormal give sharper, more geometric offsets.

### Behavior

```
Offset applied to path:

Original:     ●───────●───────●
                      |
Offset (+):   ●───────●───────●  (shifted perpendicular to path)

At corners:

Original:     ●                   With Mitre adjustment:
               \                            ●
                ●                          / \
               /                          ●   ●
              ●                          /     \
                                        ●       ●
```

### Inputs

| Pin         | Type             | Description                               |
| ----------- | ---------------- | ----------------------------------------- |
| **In**      | Points           | Path points to offset                     |
| **Filters** | Filter Factories | Filters to select which points are offset |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Offset Method</strong> <code>EPCGExOffsetMethod</code></summary>

Algorithm used to compute the offset direction at each point.

| Option         | Description                                                                                   |
| -------------- | --------------------------------------------------------------------------------------------- |
| **Slide**      | Compute offset from path normals/binormals. Smooth results, may have issues at sharp corners. |
| **Line/Plane** | Use geometric line-plane intersection. More precise at corners but can be less smooth.        |

Default: `Slide`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the offset distance.

| Option        | Description                         |
| ------------- | ----------------------------------- |
| **Constant**  | Use the constant Offset value.      |
| **Attribute** | Read offset from a point attribute. |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

The offset distance. Positive values offset in the computed direction, negative values offset in the opposite direction.

Default: `1.0`

📋 _Visible when Offset Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Point Scale To Offset</strong> <code>bool</code></summary>

Scale the offset distance and direction using each point's scale.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Vector Constant</strong> <code>FVector</code></summary>

The up vector used to calculate offset direction (defines what "perpendicular" means in 3D).

Default: World Up `(0, 0, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Type</strong> <code>EPCGExInputValueType</code></summary>

Source for the offset direction vector.

| Option        | Description                                                     |
| ------------- | --------------------------------------------------------------- |
| **Constant**  | Use the computed path direction (Normal, Binormal, or Average). |
| **Attribute** | Read direction from a point attribute.                          |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>EPCGExPathNormalDirection</code></summary>

Which path-derived direction to use for offset.

| Option             | Description                                                         |
| ------------------ | ------------------------------------------------------------------- |
| **Normal**         | The path's normal vector (perpendicular to path and up).            |
| **Binormal**       | The path's binormal vector (perpendicular to path in the up plane). |
| **Average Normal** | Average of neighboring normals for smoother results.                |

Default: `AverageNormal`

📋 _Visible when Offset Method = Slide and Direction Type = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Direction</strong> <code>bool</code></summary>

Invert the offset direction. Useful for consistent inversion regardless of the offset value's sign.

Default: `false`

⚡ PCG Overridable

</details>

***

#### Corner Adjustment (Slide Method)

<details>

<summary><strong>Adjustment</strong> <code>EPCGExOffsetAdjustment</code></summary>

How to handle tight corners where offset paths would create artifacts.

| Option            | Description                                                       |
| ----------------- | ----------------------------------------------------------------- |
| **Raw**           | No adjustment. May cause self-intersection at tight corners.      |
| **Custom Smooth** | Apply custom smoothing factor at corners.                         |
| **Auto Smooth**   | Automatically smooth based on corner angle.                       |
| **Mitre**         | Extend offset lines to intersection (like picture frame corners). |

Default: `SmoothAuto`

📋 _Visible when Offset Method = Slide_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Adjustment Scale</strong> <code>double</code></summary>

Custom smoothing factor for corner adjustment.

Default: `-0.5`

📋 _Visible when Adjustment = Custom Smooth_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mitre Limit</strong> <code>double</code></summary>

Maximum mitre extension as a multiple of the offset distance. Prevents extremely long extensions at acute angles.

Default: `4.0`

📋 _Visible when Adjustment = Mitre_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExOffsetPath.h)
