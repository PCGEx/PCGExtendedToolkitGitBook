---
description: Turns splines to paths.
icon: circle
---

# Spline to Path

### Overview

This node converts Unreal Spline data into PCG path points, extracting spline control points with their tangent information. Each spline point becomes a path point with optional attributes for tangents, length, alpha position, and point type.

### How It Works

1. **Read Splines**: Process input spline data from the Splines pin.
2. **Filter by Type**: Optionally filter to only closed loops or open splines.
3. **Extract Points**: Convert each spline control point to a path point.
4. **Write Tangents**: Optionally store arrive and leave tangent vectors as attributes.
5. **Write Metadata**: Optionally write length, alpha, and point type information.

{% hint style="warning" %}
### Not a sampler !

This node doesn't do any sampling, interpolation or transformations. It is a direct 1:1 representation of the spline control points _as PCG points_.
{% endhint %}

**Usage Notes**

* **Tangent Attributes**: The written tangent attributes can be used by downstream spline mesh nodes or for path orientation calculations.
* **Alpha Attribute**: The alpha value (0-1) represents the normalized position along the spline, useful for gradient effects or distance-based operations.
* **Point Type**: Stores the original spline interpolation type (Linear, Curve, etc.) as an integer for downstream processing.
* **Transform Inheritance**: Control whether points inherit the spline's scale and rotation or use default transforms.

### Inputs

| Pin         | Type   | Description                     |
| ----------- | ------ | ------------------------------- |
| **Splines** | Spline | Spline data to convert to paths |

### Outputs

| Pin     | Type   | Description                        |
| ------- | ------ | ---------------------------------- |
| **Out** | Points | Path points extracted from splines |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Transform Details</strong> <code>FPCGExLeanTransformDetails</code></summary>

Controls how point transforms are derived from the spline.

| Property             | Description                            | Default |
| -------------------- | -------------------------------------- | ------- |
| **Inherit Scale**    | Apply the spline's scale to points.    | `true`  |
| **Inherit Rotation** | Apply the spline's rotation to points. | `true`  |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Inputs</strong> <code>EPCGExSplineSamplingIncludeMode</code></summary>

Which splines to convert.

| Option                | Description                                      |
| --------------------- | ------------------------------------------------ |
| **All**               | Convert all splines.                             |
| **Closed Loops Only** | Only convert splines that are closed loops.      |
| **Open Lines Only**   | Only convert splines that are open (not looped). |

Default: `All`

⚡ PCG Overridable

</details>

***

#### Tangent Attributes

<details>

<summary><strong>Write Arrive Tangent</strong> <code>bool</code></summary>

Write the spline's arrive tangent (incoming direction) to an FVector attribute.

Default: `true`

Attribute: `ArriveTangent`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Leave Tangent</strong> <code>bool</code></summary>

Write the spline's leave tangent (outgoing direction) to an FVector attribute.

Default: `true`

Attribute: `LeaveTangent`

⚡ PCG Overridable

</details>

***

#### Metadata Attributes

<details>

<summary><strong>Write Length At Point</strong> <code>bool</code></summary>

Write the cumulative spline length at each point. This is the distance traveled along the spline from the start to this point.

Default: `false`

Attribute: `LengthAtPoint`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Alpha</strong> <code>bool</code></summary>

Write the normalized position along the spline (0 at start, 1 at end).

Default: `false`

Attribute: `Alpha`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Point Type</strong> <code>bool</code></summary>

Write the original spline point interpolation type as an integer.

* `0` = Linear
* `1` = Curve
* `2` = Constant
* `3` = CurveClamped
* `4` = CurveCustomTangent

Default: `false`

Attribute: `PointType`

⚡ PCG Overridable

</details>

***

#### Tagging

<details>

<summary><strong>Tags To Data</strong> <code>EPCGExTagsToDataAction</code></summary>

How to handle tags from source splines.

| Option         | Description                      |
| -------------- | -------------------------------- |
| **Do Nothing** | Ignore source tags.              |
| **To @Data**   | Forward tags to the data domain. |
| **Attribute**  | Store tags as point attributes.  |

Default: `To @Data`

</details>

<details>

<summary><strong>Tag Forwarding</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filter which tags are forwarded from source splines.

//→ See TODO FPCGExNameFiltersDetails

</details>

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are preserved during conversion.

//→ See TODO FPCGExCarryOverDetails

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExSplineToPath.h)
