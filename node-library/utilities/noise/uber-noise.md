---
description: Generate noise or mutate existing attribute using noises.
icon: circle-plus
---

# Uber Noise

### Overview

This node applies noise generation to point data using connected noise sub-nodes. It can either create new attributes filled with noise values, or blend noise with existing attributes using configurable blend modes. Multiple noise types can be combined through the noise factory system.

### How It Works

1. **Gather Noise**: Collects noise generator factories from connected sub-nodes.
2. **Sample Noise**: For each point, samples noise at the point's position.
3. **Apply Result**: Either writes noise as a new attribute or blends with an existing attribute.

**Usage Notes**

* **Noise Sub-Nodes**: Connect noise generator sub-nodes (Perlin, Simplex, Worley, etc.) to define the noise.
* **New vs Mutate**: "New Attribute" creates fresh values; "Mutate Attribute" blends noise with existing values.
* **Blend Modes**: When mutating, choose how noise combines with the source (Add, Multiply, Lerp, etc.).
* **Weight**: Control the influence of the source value when blending.

### Behavior

```
Noise Generation:

Mode = New Attribute:
   Creates attribute "Noise" with sampled noise values
   Point A at (100, 200, 0): Noise = 0.73
   Point B at (150, 250, 0): Noise = 0.45

Mode = Mutate Attribute:
   Blends noise with existing "Density" values
   Point A: Density 0.5 + Noise 0.3 = 0.8 (Add blend)
   Point B: Density 0.8 * Noise 0.5 = 0.4 (Multiply blend)
```

### Inputs

| Pin         | Type   | Description                                                |
| ----------- | ------ | ---------------------------------------------------------- |
| **In**      | Points | Input point collections                                    |
| **Filters** | Params | Optional point filters to limit which points receive noise |
| **Noises**  | Params | Noise generator sub-nodes defining the noise to apply      |

### Settings

#### Mode Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExUberNoiseMode</code></summary>

Determines whether to create new values or blend with existing attributes.

| Option               | Description                              |
| -------------------- | ---------------------------------------- |
| **New Attribute**    | Create a new attribute with noise values |
| **Mutate Attribute** | Blend noise with an existing attribute   |

Default: `New Attribute`

</details>

<details>

<summary><strong>Output Type</strong> <code>EPCGMetadataTypes</code></summary>

The data type for the new attribute when creating new values.

Default: `Double`

📋 _Visible when Mode = New Attribute_

</details>

#### Attribute Settings

<details>

<summary><strong>Source</strong> <code>FName</code></summary>

The attribute to read from (Mutate mode) or the default name for new attributes.

Default: `$Density`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output To Different Name</strong> <code>bool</code></summary>

When enabled, writes output to a different attribute name.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target</strong> <code>FName</code></summary>

The attribute name to write results to.

📋 _Visible when Output To Different Name is enabled_

⚡ PCG Overridable

</details>

#### Blend Settings (Mutate Mode)

<details>

<summary><strong>Blend Mode</strong> <code>EPCGExABBlendingType</code></summary>

How noise values are combined with the source attribute.

| Option       | Description                      |
| ------------ | -------------------------------- |
| **Add**      | Source + Noise                   |
| **Subtract** | Source - Noise                   |
| **Multiply** | Source × Noise                   |
| **Divide**   | Source ÷ Noise                   |
| **Min**      | Minimum of Source and Noise      |
| **Max**      | Maximum of Source and Noise      |
| **Average**  | (Source + Noise) / 2             |
| **Lerp**     | Interpolate using weight         |
| **Weight**   | Weighted blend                   |
| ...          | Additional blend modes available |

Default: `Add`

📋 _Visible when Mode = Mutate Attribute_

</details>

<details>

<summary><strong>Source Value Weight</strong> <code>FPCGExInputShorthandSelectorDouble</code></summary>

Weight applied to the source value during blending. Can be constant or read from an attribute.

| Property      | Description                   |
| ------------- | ----------------------------- |
| **Input**     | Constant or Attribute         |
| **Constant**  | Fixed weight value            |
| **Attribute** | Attribute to read weight from |

Default: `1.0` (constant)

📋 _Visible when Mode = Mutate Attribute_

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                             |
| ------- | ------ | ------------------------------------------------------- |
| **Out** | Points | Points with noise-generated or noise-blended attributes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExUberNoise.h)
