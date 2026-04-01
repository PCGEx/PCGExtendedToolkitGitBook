---
description: Extrude input points into paths along tensors.
icon: circle-plus
---

# Path : Extrude Tensors

### Overview

Extrude Tensors generates paths by iteratively moving seed points along tensor field directions. Each seed point becomes the starting point of a path that follows the combined tensor influence, creating streamlines through the field. The extrusion can be controlled by iteration limits, length constraints, and intersection detection with other paths.

### How It Works

1. **Tensor Setup**: Loads tensor factories and prepares the tensor field for sampling.
2. **Seed Processing**: For each input seed point, initializes an extrusion state.
3. **Iterative Extrusion**: Repeatedly samples the tensor field at the current position and moves along the resulting direction.
4. **Limit Checking**: Tests against maximum iterations, length, and point count limits.
5. **Intersection Detection**: Optionally checks for intersections with external paths or other extruding paths.
6. **Path Output**: Collects extruded points into path data with optional tagging.

**Usage Notes### Sampler selection**

The choice of tensor sampler significantly affects output quality. The **Adaptive RK** sampler typically produces the best results in curved regions by dynamically adjusting step size based on field curvature, though it's slightly slower than the default. For complex fields with varying curvature, Adaptive RK is recommended.- **Child Extrusions**: When an extrusion is stopped by filters but later exits the stop region, a new "child" path can optionally continue.

* **Self-Intersection**: When enabled, paths that cross each other can be detected and stopped, with configurable priority rules.
* **Closed Loops**: The node can detect when a path curves back toward its origin and close the loop automatically.

### Behavior

```
Seed Points + Tensor Field:

     ●seed                    ●────●────●────●────●  (path following field)
        ↘                          ↘
    ●seed → Tensor Field  →   ●────●────●────●
        ↗                          ↗
     ●seed                    ●────●────●────●────●────●
```

### Inputs

| Pin              | Type             | Description                                                  |
| ---------------- | ---------------- | ------------------------------------------------------------ |
| **In**           | Point Data       | Seed points to extrude                                       |
| **Tensors**      | Tensor Factories | Tensor field definitions controlling extrusion direction     |
| **Stop Filters** | Filter Factories | Optional filters that stop extrusion when conditions are met |
| **Paths**        | Point Data       | Optional external paths for intersection testing             |

{% hint style="warning" %}
### Only spatial filters that do not rely on point attributes will work as stop conditions.

Points generated during extrusion exist only in world space and do not have access to the same attributes as your input data. Use filters based on position, bounds, or other spatial criteria (e.g., Bounds Filter, Inclusion Filter, Distance Filter).
{% endhint %}

### Settings

#### Transform Settings

<details>

<summary><strong>Transform Rotation</strong> <code>bool</code></summary>

When enabled, updates point rotations during extrusion to align with movement direction.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation</strong> <code>EPCGExTensorTransformMode</code></summary>

How rotation transform is applied to extruded points.

| Option       | Description                                       |
| ------------ | ------------------------------------------------- |
| **Absolute** | Set rotation to the tensor direction              |
| **Relative** | Add tensor rotation to existing rotation          |
| **Align**    | Align a specific axis with the movement direction |

Default: `Align`

📋 _Visible when Transform Rotation is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Align Axis</strong> <code>EPCGExAxis</code></summary>

Which local axis to align with the movement direction.

Default: `Forward`

📋 _Visible when Transform Rotation is enabled and Rotation = Align_

⚡ PCG Overridable

</details>

#### Iteration Settings

<details>

<summary><strong>Use Per Point Max Iterations</strong> <code>bool</code></summary>

When enabled, reads the maximum iteration count from a per-point attribute instead of using a global value.

Default: `false`

</details>

<details>

<summary><strong>Per-point Iterations</strong> <code>FName</code></summary>

Attribute name to read iteration count from.

Default: `Iterations`

📋 _Visible when Use Per Point Max Iterations is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Iterations</strong> <code>int32</code></summary>

Maximum number of extrusion steps per seed. Acts as a clamp when using per-point values.

Default: `1`

⚡ PCG Overridable

</details>

#### Limit Settings

<details>

<summary><strong>Use Max Length</strong> <code>bool</code></summary>

When enabled, limits the total length of generated paths.

Default: `false`

</details>

<details>

<summary><strong>Max Length</strong> <code>double</code></summary>

Maximum path length before extrusion stops.

Default: `100`

📋 _Visible when Use Max Length is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Max Points Count</strong> <code>bool</code></summary>

When enabled, limits the number of points in each path.

Default: `false`

</details>

<details>

<summary><strong>Max Points Count</strong> <code>int32</code></summary>

Maximum number of points per path.

Default: `100`

📋 _Visible when Use Max Points Count is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fuse Distance</strong> <code>double</code></summary>

Minimum distance between consecutive points. Points closer than this are fused.

Default: `DBL_COLLOCATION_TOLERANCE`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Stop Condition Handling</strong> <code>EPCGExTensorStopConditionHandling</code></summary>

How to handle points that trigger stop conditions.

| Option      | Description                                   |
| ----------- | --------------------------------------------- |
| **Exclude** | Don't include the stopping point in the path  |
| **Include** | Include the stopping point as the final point |

Default: `Exclude`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Child Extrusions</strong> <code>bool</code></summary>

When enabled, if an extrusion exits stop conditions after being stopped, a new child path can continue from that point.

Default: `false`

⚡ PCG Overridable

</details>

#### External Intersection Settings

<details>

<summary><strong>Do External Path Intersections</strong> <code>bool</code></summary>

When enabled, tests for intersections with paths provided on the Paths input pin.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>External Path Intersections</strong> <code>FPCGExPathIntersectionDetails</code></summary>

Settings for external path intersection detection including tolerance and angle constraints.

📋 _Visible when Do External Path Intersections is enabled_

⚡ PCG Overridable

</details>

#### Self Intersection Settings

<details>

<summary><strong>Do Self Path Intersections</strong> <code>bool</code></summary>

When enabled, tests for intersections between actively extruding paths.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Self Intersection Mode</strong> <code>EPCGExSelfIntersectionMode</code></summary>

How to determine which path has priority at intersections.

| Option           | Description                                      |
| ---------------- | ------------------------------------------------ |
| **Path Length**  | Longer/shorter path wins based on sort direction |
| **Sorting Only** | Use sorting rules exclusively                    |

Default: `PathLength`

📋 _Visible when Do Self Path Intersections is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Merge On Proximity</strong> <code>bool</code></summary>

When enabled, paths that come close together can merge instead of just stopping.

Default: `false`

📋 _Visible when Do Self Path Intersections is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Detect Closed Loops</strong> <code>bool</code></summary>

When enabled, detects when a path curves back toward its origin and can close the loop.

Default: `false`

</details>

#### Tagging Settings

<details>

<summary><strong>Attributes To Path Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copy source point attributes as tags on output paths.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Tag If Child Extrusion</strong> <code>bool</code></summary>

Tag paths that were spawned as children of another extrusion.

Default: `false`

</details>

<details>

<summary><strong>Tag If Is Stopped By Filters</strong> <code>bool</code></summary>

Tag paths that were stopped by stop filter conditions.

Default: `false`

</details>

<details>

<summary><strong>Tag If Is Stopped By Intersection</strong> <code>bool</code></summary>

Tag paths that were stopped by external path intersection.

Default: `false`

</details>

#### Tensor Sampling Settings

<details>

<summary><strong>Tensor Sampling Settings</strong> <code>FPCGExTensorHandlerDetails</code></summary>

Settings for how tensors are sampled and combined.

| Property             | Type     | Description                              |
| -------------------- | -------- | ---------------------------------------- |
| **Invert**           | `bool`   | Invert the tensor direction              |
| **Normalize**        | `bool`   | Normalize the result to unit length      |
| **Size**             | `double` | Scale factor for movement distance       |
| **Uniform Scale**    | `double` | Additional uniform scaling               |
| **Sampler Settings** | Struct   | Tensor sampler configuration (see below) |

**Available Tensor Samplers:**

| Sampler         | Description                                                                                                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Default**     | Simple single-point sampling. Fast but may miss detail in curved fields.                                                                                                               |
| **Adaptive RK** | Dynamically adjusts step size based on field curvature. Takes smaller steps in curved regions, larger steps in smooth regions. Best overall quality. _Recommended for most use cases._ |
| **RK4**         | Uses the Runge-Kutta 4 method for higher-order integration accuracy. Good for smooth, predictable fields.                                                                              |
| **Six Points**  | Samples six points around the target location and averages results. Helps smooth out noise in the field.                                                                               |

⚡ PCG Overridable

</details>

#### Output Settings

<details>

<summary><strong>Refresh Seed</strong> <code>bool</code></summary>

When enabled, gives new random seeds to output points. Otherwise, they inherit from the source.

Default: `true`

</details>

<details>

<summary><strong>Paths Output Settings</strong> <code>FPCGExPathOutputDetails</code></summary>

Settings for filtering output paths by point count.

//→ See TODO FPCGExPathOutputDetails

</details>

#### Inherited Settings

This node inherits from the path processor base.

→ See Path Processor Settings for common path processing options.

### Outputs

| Pin     | Type       | Description                               |
| ------- | ---------- | ----------------------------------------- |
| **Out** | Point Data | Generated path data from tensor extrusion |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Elements/PCGExExtrudeTensors.h)
