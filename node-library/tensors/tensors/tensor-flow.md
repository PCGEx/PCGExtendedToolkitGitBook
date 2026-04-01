---
description: A tensor that represents a vector/flow field.
icon: circle-dashed
---

# Tensor : Flow

### Overview

The Flow Tensor creates a directional field from input points, where each point contributes a direction vector based on its transform or a specified attribute. This allows you to define flow patterns by placing and orienting guide points — the tensor field interpolates between them to create smooth directional flows. Sampling points receive direction influence from nearby flow sources based on distance falloff.

### How It Works

1. **Source Setup**: Reads direction vectors from input points, either from their transform axes or a vector attribute.
2. **Effector Array**: Builds an array of flow effectors, each with position, direction, and influence settings.
3. **Spatial Query**: When sampled, finds nearby flow sources within influence range.
4. **Direction Blending**: Combines directions from all contributing sources using weight and potency falloff curves.

**Usage Notes**

* **Point-Based Flow**: Flow directions are defined by input points — arrange and orient points to sculpt the flow field.
* **Attribute Direction**: Use a vector attribute for arbitrary directions, or use transform axes for orientation-based flows.
* **Falloff Control**: The potency and weight falloff curves control how quickly influence decreases with distance from source points.

### Behavior

```
Input Points with Forward directions:

  ●→         ●→         ●→
     ↘        →        ↗
        ↘    →    ↗
           →→→→
        ↗    →    ↘
     ↗        →        ↘
  ●→         ●→         ●→

Sampled directions blend between nearby
source point directions based on distance.
```

### Inputs

| Pin    | Type       | Description                                         |
| ------ | ---------- | --------------------------------------------------- |
| **In** | Point Data | Points defining flow directions and their positions |

### Settings

#### Direction Settings

<details>

<summary><strong>Direction Input</strong> <code>EPCGExInputValueType</code></summary>

How to determine the direction vector for each input point.

| Option        | Description                                     |
| ------------- | ----------------------------------------------- |
| **Constant**  | Use a transform axis from each point's rotation |
| **Attribute** | Read direction from a vector attribute          |

Default: `Attribute`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction (Constant)</strong> <code>EPCGExAxis</code></summary>

Direction axis read from the input points' transform.

| Option       | Description           |
| ------------ | --------------------- |
| **Forward**  | Local X axis          |
| **Backward** | Negative local X axis |
| **Right**    | Local Y axis          |
| **Left**     | Negative local Y axis |
| **Up**       | Local Z axis          |
| **Down**     | Negative local Z axis |

Default: `Forward`

📋 _Visible when Direction Input = Constant_

</details>

<details>

<summary><strong>Direction (Attribute)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute selector for the direction vector.

Default: `$Rotation.Forward`

📋 _Visible when Direction Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Direction</strong> <code>bool</code></summary>

When enabled, flips the direction vector (multiplies by -1).

Default: `false`

📋 _Visible when Direction Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Transform</strong> <code>EPCGExTransformMode</code></summary>

Whether the direction is in world space or should be transformed by the point's rotation.

| Option       | Description                                |
| ------------ | ------------------------------------------ |
| **Absolute** | Direction is in world space                |
| **Relative** | Direction is transformed by point rotation |

Default: `Relative`

📋 _Visible when Direction Input = Attribute_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits from the tensor point factory provider base, which includes weight, potency, and falloff curve settings.

→ See Tensor Factory for common tensor options including Weight, Potency, and Falloff Curves.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorFlow.h)
