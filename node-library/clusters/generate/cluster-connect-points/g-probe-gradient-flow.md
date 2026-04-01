---
description: Connect points following attribute gradient direction.
icon: circle-dashed
---

# G-Probe : Gradient Flow

### Overview

This global probe creates connections based on the gradient (rate of change) of an attribute value. Points connect to neighbors that represent "uphill" or "downhill" movement along the attribute's gradient. This creates flow-like connectivity patterns, similar to water flowing downhill or heat diffusing toward cooler areas.

<figure><img src="../../../../.gitbook/assets/image (287).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Attribute Reading**: Loads the specified flow attribute value for each point
2. **Neighbor Search**: For each point, finds neighbors within the search radius
3. **Gradient Evaluation**: Compares attribute values between the point and its neighbors
4. **Connection Creation**: Creates edges following the gradient direction (toward higher or lower values)

**Usage Notes**

* **Flow Direction**: By default, connects to both higher and lower values. Enable "Uphill Only" to restrict flow to increasing values only
* **Steepest Path**: When enabled (default), each point connects only to the neighbor with the greatest attribute difference, creating distinct flow paths rather than a dense mesh
* **Common Attributes**: Height/Z for terrain drainage, density for diffusion patterns, temperature for heat flow, or custom scalar attributes

### Behavior

```
Gradient Flow with attribute values:

    Points with values:        Steepest Only (default):    All Neighbors:

       [3]   [5]   [2]            [3]───[5]   [2]           [3]───[5]───[2]
        │     │     │              │     │                   │ ╲   │   ╱ │
       [4]   [7]   [1]            [4]   [7]   [1]           [4]───[7]───[1]
        │     │     │              │     │     │             │ ╱   │   ╲ │
       [2]   [6]   [3]            [2]   [6]   [3]           [2]───[6]───[3]
                                   └─────┘

    Flow follows gradient     Each point → steepest      All gradient neighbors
    from low to high values   neighbor only              connected
```

### Settings

#### Gradient Configuration

<details>

<summary><strong>Flow Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The scalar attribute whose gradient determines connection direction. Points will connect to neighbors based on this attribute's value differences.

Default: `$Density`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Uphill Only</strong> <code>bool</code></summary>

When enabled, only creates connections toward neighbors with higher attribute values. When disabled, connections can go both "uphill" (toward higher values) and "downhill" (toward lower values).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Steepest Only</strong> <code>bool</code></summary>

When enabled, each point connects only to the single neighbor with the greatest attribute difference (steepest gradient). This creates distinct flow paths. When disabled, connects to all neighbors that satisfy the gradient direction requirement.

Default: `true`

⚡ PCG Overridable

</details>

#### Search Radius (Inherited)

<details>

<summary><strong>Search Radius Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the search radius is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the radius from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Search Radius</strong> <code>double</code></summary>

The maximum distance to search for neighbors when evaluating gradient connections.

Default: `100`

📋 _Visible when Search Radius Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Additional offset added to the search radius value.

Default: `0`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory to connect to Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeGradientFlow.h)
