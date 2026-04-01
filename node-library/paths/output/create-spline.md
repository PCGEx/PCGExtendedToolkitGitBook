---
description: Create splines from input points.
icon: circle
---

# Create Spline

### Overview

This node converts path point data into Unreal spline components or spline data. Each input path becomes a spline, with configurable point interpolation types and optional custom tangents.

### How It Works

1. **Process Paths**: Each input path is converted to a spline with one spline point per path point.
2. **Set Point Types**: Spline points use the default interpolation type or read per-point types from an attribute.
3. **Apply Tangents**: For curve types with custom tangents, arrive/leave tangents are computed or read from attributes.
4. **Output**: Depending on mode, outputs spline data, spawns spline components, or both.

**Usage Notes**

* **Closed Loops**: Paths marked as closed loops produce closed splines.
* **Point Types**: Linear creates straight segments, Curve types create smooth interpolation with automatic or custom tangents.
* **Target Actor**: When spawning components, they attach to the specified target actor.

### Inputs

| Pin    | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| **In** | Points | Path points to convert to splines |

### Outputs

| Pin         | Type   | Description             |
| ----------- | ------ | ----------------------- |
| **Splines** | Spline | The created spline data |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGCreateSplineMode</code></summary>

How splines are created and output.

| Option                        | Description                                     |
| ----------------------------- | ----------------------------------------------- |
| **Create Data Only**          | Output spline data without spawning components. |
| **Create Component**          | Spawn spline components on the target actor.    |
| **Create Data and Component** | Both output data and spawn components.          |

Default: `CreateDataOnly`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Point Type</strong> <code>EPCGExSplinePointType</code></summary>

The interpolation type for spline points.

| Option                   | Description                                        |
| ------------------------ | -------------------------------------------------- |
| **Linear**               | Straight lines between points.                     |
| **Curve**                | Smooth curves with automatic tangents.             |
| **Constant**             | Step function (no interpolation between points).   |
| **Curve Clamped**        | Curves with clamped tangents to prevent overshoot. |
| **Curve Custom Tangent** | Curves using custom tangent attributes.            |

Default: `Linear`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Custom Point Type</strong> <code>bool</code></summary>

Read the spline point type from an attribute, allowing different interpolation per point.

Default: `false`

Attribute: `PointType`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tangents</strong> <code>FPCGExTangentsDetails</code></summary>

Settings for custom tangent computation when using `CurveCustomTangent` point type.

📋 _Visible when Default Point Type = CurveCustomTangent_

⚡ PCG Overridable

</details>

***

#### Advanced Settings

<details>

<summary><strong>Target Actor</strong> <code>TSoftObjectPtr&#x3C;AActor></code></summary>

The actor to spawn spline components on. If not set, uses the PCG component's owning actor.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Post Process Function Names</strong> <code>TArray&#x3C;FName></code></summary>

Functions to call on the target actor after spline creation. Functions must be parameter-less with "CallInEditor" flag enabled.

</details>

<details>

<summary><strong>Attachment Rules</strong> <code>FPCGExAttachmentRules</code></summary>

Rules for how spline components attach to their parent actor.

//→ See TODO FPCGExAttachmentRules

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExCreateSpline.h)
