---
description: Sample a tensor at point location and probe in that direction.
icon: circle-dashed
---

# Probe : Tensor

### Overview

The Tensor Probe samples the tensor field at each point's location and probes for connection candidates in that direction. This enables tensor-guided connectivity where points connect along flow lines, following the field rather than using fixed directions. Unlike the static Direction probe, the tensor direction can vary smoothly across space.

### How It Works

1. **Tensor Sampling**: At each point, samples the tensor field to get a direction vector.
2. **Angle Filtering**: Filters candidates to those within the configured angle threshold of the tensor direction.
3. **Candidate Scoring**: Scores candidates based on distance and/or dot product with the tensor direction.
4. **Connection Selection**: Selects the best candidate(s) based on the prioritization setting.

**Usage Notes**

* **Dynamic Directions**: Unlike Direction probe which uses a fixed or attribute-based direction, Tensor probe directions vary based on the field at each point's location.
* **Chained Processing**: When enabled, processes candidates sequentially, which can yield different results than parallel processing.
* **Component-Wise Angles**: Allows different tolerances for pitch, yaw, and roll instead of a single cone angle.

### Behavior

```
Tensor Field: Flowing right-to-left in a curve

Point A (tensor = →):     Probes right, finds B
Point B (tensor = ↗):     Probes up-right, finds C
Point C (tensor = ↑):     Probes up, finds D

Result: A──B──C──D follows the tensor flow
```

### Inputs

| Pin         | Type             | Description                                   |
| ----------- | ---------------- | --------------------------------------------- |
| **Tensors** | Tensor Factories | Tensor field definitions for probe directions |

### Settings

<details>

<summary><strong>Invert Tensor Direction</strong> <code>bool</code></summary>

When enabled, mirrors the direction sampled from tensors (\*-1). Probes in the opposite direction of the tensor field.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Favor</strong> <code>EPCGExProbeDirectionPriorization</code></summary>

What to prioritize when selecting the best candidate.

| Option       | Description                                             |
| ------------ | ------------------------------------------------------- |
| **Dot**      | Favor candidates most aligned with the tensor direction |
| **Distance** | Favor closest candidates within the angle range         |

Default: `Dot`

</details>

<details>

<summary><strong>Use Component Wise Angle</strong> <code>bool</code></summary>

When enabled, uses separate angle thresholds for pitch, yaw, and roll instead of a single max angle cone.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angle</strong> <code>double</code></summary>

Maximum angle (in degrees) from the tensor direction within which to search for candidates.

Default: `45`

Range: `0` to `180`

📋 _Visible when Use Component Wise Angle is disabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angles</strong> <code>FRotator</code></summary>

Maximum angles per component (Pitch, Yaw, Roll) within which to search for candidates.

Default: `(45, 45, 45)`

📋 _Visible when Use Component Wise Angle is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Do Chained Processing</strong> <code>bool</code></summary>

When enabled, processes candidates sequentially after other probes. Can yield different results than parallel processing.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tensor Sampling Settings</strong> <code>FPCGExTensorHandlerDetails</code></summary>

Settings for how tensors are sampled at each point.

| Property             | Type     | Description                          |
| -------------------- | -------- | ------------------------------------ |
| **Invert**           | `bool`   | Invert the tensor direction          |
| **Normalize**        | `bool`   | Normalize the tensor result          |
| **Size**             | `double` | Scale factor (affects search radius) |
| **Sampler Settings** | Struct   | Tensor sampler configuration         |

⚡ PCG Overridable

</details>

#### Inherited Settings

This probe inherits from the probe factory base.

→ See Probe Factory for common probe options including search radius and connection limits.

### Outputs

| Pin       | Type          | Description                                  |
| --------- | ------------- | -------------------------------------------- |
| **Probe** | Probe Factory | Probe definition for use with Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Probes/PCGExProbeTensor.h)
