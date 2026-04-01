---
description: Configures how tangent vectors are sourced and scaled for spline operations.
icon: sliders-simple
---

# Tangents Details

### Overview

This settings block controls tangent data for spline-based operations like spline mesh generation. Tangents define the curve direction at each point, creating smooth interpolation between points. Tangents can be read from existing attributes or calculated in-place using various algorithms, with optional scaling for fine-tuning curve behavior.

### How It Works

1. **Select Source**: Choose where tangent data comes from (attribute or in-place calculation)
2. **Read/Calculate**: Either read tangent vectors from attributes or compute them
3. **Apply Scaling**: Optionally scale arrive and leave tangents independently
4. **Use in Spline**: Tangent data controls curve shape between points

### Behavior

```
Tangent Sources:

No Tangents: Linear interpolation between points
  ●─────────●─────────●

Attribute: Read pre-computed tangent vectors
  Arrive ← ● → Leave
           │
  ●───────────────────●

In-Place: Calculate tangents using selected algorithm
  (Catmull-Rom, Auto, Neighbors, etc.)

Scaling Effect:
  Scale < 1: Tighter curves, sharper corners
  Scale = 1: Default curve tension
  Scale > 1: Looser curves, more overshoot
```

### Settings

<details>

<summary><strong>Source</strong> <code>EPCGExTangentSource</code></summary>

Determines where tangent data comes from.

| Option          | Description                                |
| --------------- | ------------------------------------------ |
| **No Tangents** | No tangent data used; linear interpolation |
| **Attribute**   | Read tangent vectors from point attributes |
| **In-Place**    | Calculate tangents using a tangent factory |

Default: `Attribute`

</details>

<details>

<summary><strong>Arrive Tangent Attribute</strong> <code>FName</code></summary>

Name of the attribute containing arrive (incoming) tangent vectors.

Default: `ArriveTangent`

📋 _Visible when Source = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Leave Tangent Attribute</strong> <code>FName</code></summary>

Name of the attribute containing leave (outgoing) tangent vectors.

Default: `LeaveTangent`

📋 _Visible when Source = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tangents</strong> <code>UPCGExTangentsInstancedFactory</code></summary>

Factory sub-node used to calculate tangents in-place. Different factories provide different calculation methods (Catmull-Rom, Auto, etc.).

📋 _Visible when Source = In-Place_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scaling</strong> <code>FPCGExTangentsScalingDetails</code></summary>

Controls scaling applied to tangent vectors.

| Sub-Setting      | Description                          |
| ---------------- | ------------------------------------ |
| **Arrive Scale** | Multiplier for arrive tangent length |
| **Leave Scale**  | Multiplier for leave tangent length  |

Each can be a constant or read from an attribute.

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Tangents/PCGExTangentsInstancedFactory.h)
