---
description: Transform input points using tensors.
icon: circle-plus
---

# Tensors Transform

### Overview

Tensors Transform moves and rotates input points based on tensor field sampling. Unlike Extrude Tensors which creates paths, this node transforms points in place — each point samples the tensor field at its position and moves accordingly. Multiple iterations can be applied to progressively move points through the field.

### How It Works

1. **Tensor Setup**: Loads tensor factories and prepares the combined tensor field.
2. **Iteration Loop**: For each iteration:
   * Samples the tensor field at each point's current position
   * Updates position based on the sampled direction and magnitude
   * Optionally updates rotation to align with movement
3. **Stop Condition Check**: Points matching stop filter conditions cease transforming.
4. **Output Attributes**: Optionally writes diagnostic data about the transformation process.

**Usage Notes### Sampler selection**

The choice of tensor sampler significantly affects output quality. The **Adaptive RK** sampler typically produces the best results in curved regions by dynamically adjusting step size based on field curvature, though it's slightly slower than the default. For complex fields with varying curvature, Adaptive RK is recommended.

* **Single vs Multiple Iterations**: With one iteration, points move once based on their starting position. Multiple iterations create cumulative movement as points sample from their new positions.
* **Stop Filters**: Allow selective stopping — points in certain regions can halt transformation while others continue.

### Behavior

```
Input Points + Tensor Field:

   ●A ●B ●C              ●A'    ●B'    ●C'
                    →    (moved along tensor directions)

Iterations = 3:
   ●start → ●iter1 → ●iter2 → ●final
```

### Inputs

| Pin              | Type             | Description                                                   |
| ---------------- | ---------------- | ------------------------------------------------------------- |
| **In**           | Point Data       | Points to transform                                           |
| **Tensors**      | Tensor Factories | Tensor field definitions controlling transformation           |
| **Stop Filters** | Filter Factories | Optional filters that stop transformation for matching points |

### Settings

#### Transform Settings

<details>

<summary><strong>Transform Position</strong> <code>bool</code></summary>

When enabled, updates point positions using tensor sampling.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform Rotation</strong> <code>bool</code></summary>

When enabled, updates point rotations using tensor sampling.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation</strong> <code>EPCGExTensorTransformMode</code></summary>

How rotation transform is applied.

| Option       | Description                                                           |
| ------------ | --------------------------------------------------------------------- |
| **Absolute** | Set rotation directly from tensor direction, ignoring source rotation |
| **Relative** | Add tensor rotation to existing rotation                              |
| **Align**    | Align a specific axis with the movement direction                     |

Default: `Align`

📋 _Visible when Transform Rotation is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Align Axis</strong> <code>EPCGExAxis</code></summary>

Which local axis to align with the movement direction.

| Option       | Description              |
| ------------ | ------------------------ |
| **Forward**  | Align forward (+X) axis  |
| **Backward** | Align backward (-X) axis |
| **Right**    | Align right (+Y) axis    |
| **Left**     | Align left (-Y) axis     |
| **Up**       | Align up (+Z) axis       |
| **Down**     | Align down (-Z) axis     |

Default: `Forward`

📋 _Visible when Transform Rotation is enabled and Rotation = Align_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Iterations</strong> <code>int32</code></summary>

Number of iterations to apply tensor transforms. Each iteration samples from the point's current position after previous iterations.

Default: `1`

⚡ PCG Overridable

</details>

#### Limit Settings

<details>

<summary><strong>Stop Condition Handling</strong> <code>EPCGExTensorStopConditionHandling</code></summary>

How to handle points that match stop filter conditions.

| Option      | Description                                              |
| ----------- | -------------------------------------------------------- |
| **Exclude** | Remove points that are stopped from the output           |
| **Include** | Keep stopped points in the output at their last position |

Default: `Exclude`

⚡ PCG Overridable

</details>

#### Output Attributes

<details>

<summary><strong>Write Effectors Pings</strong> <code>bool</code></summary>

When enabled, writes the total number of tensor effectors that affected each point across all iterations.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Effectors Pings</strong> <code>FName</code></summary>

Attribute name for effector ping count (`int32`).

Default: `EffectorsPings`

📋 _Visible when Write Effectors Pings is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Update Count</strong> <code>bool</code></summary>

When enabled, writes how many iterations actually affected each point before stopping.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Update Count</strong> <code>FName</code></summary>

Attribute name for update count (`int32`).

Default: `UpdateCount`

📋 _Visible when Write Update Count is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Traveled Distance</strong> <code>bool</code></summary>

When enabled, writes the approximate total distance traveled by each point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Traveled Distance</strong> <code>FName</code></summary>

Attribute name for traveled distance (`double`).

Default: `TraveledDistance`

📋 _Visible when Write Traveled Distance is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Gracefully Stopped</strong> <code>bool</code></summary>

When enabled, writes whether each point stopped before reaching max iterations (due to stop filters or zero tensor).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Gracefully Stopped</strong> <code>FName</code></summary>

Attribute name for graceful stop flag (`bool`).

Default: `GracefullyStopped`

📋 _Visible when Write Gracefully Stopped is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Max Iterations Reached</strong> <code>bool</code></summary>

When enabled, writes whether each point completed all iterations without stopping early.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Iterations Reached</strong> <code>FName</code></summary>

Attribute name for max iterations flag (`bool`).

Default: `MaxIterationsReached`

📋 _Visible when Write Max Iterations Reached is enabled_

⚡ PCG Overridable

</details>

#### Tensor Sampling Settings

<details>

<summary><strong>Tensor Sampling Settings</strong> <code>FPCGExTensorHandlerDetails</code></summary>

Settings for how tensors are sampled and combined.

| Property             | Type                   | Default    | Description                                          |
| -------------------- | ---------------------- | ---------- | ---------------------------------------------------- |
| **Invert**           | `bool`                 | `false`    | Invert the tensor direction                          |
| **Normalize**        | `bool`                 | `true`     | Normalize result to unit length before applying size |
| **Size Input**       | `EPCGExInputValueType` | `Constant` | Constant or attribute for movement size              |
| **Size**             | `double`               | `100`      | Movement distance per iteration                      |
| **Uniform Scale**    | `double`               | `1`        | Additional uniform scaling factor                    |
| **Sampler Settings** | Struct                 | -          | Tensor sampler configuration                         |

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                            |
| ------- | ---------- | ------------------------------------------------------ |
| **Out** | Point Data | Transformed points with optional diagnostic attributes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Elements/PCGExTensorsTransform.h)
