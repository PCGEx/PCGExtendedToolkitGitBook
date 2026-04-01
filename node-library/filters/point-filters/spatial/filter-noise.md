---
description: Compare a value against spatial noise.
icon: circle-dashed
---

# Filter : Noise

### Overview

This filter evaluates spatial noise at each point's position and compares the result against a threshold value. Points pass or fail the filter based on the comparison result. Connected noise sub-nodes define the type and configuration of noise sampled at each point location.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Sample Noise**: Evaluates noise at each point's world position using connected noise generators.
2. **Get Threshold**: Reads the comparison value from constant or attribute.
3. **Compare Values**: Applies the comparison operator to determine pass/fail.

**Usage Notes**

* **Noise Sub-Nodes**: Connect noise generator sub-nodes (Perlin, Simplex, Worley, etc.) to define the noise type.
* **Threshold Value**: Can be constant or read from an attribute for per-point thresholds.
* **Spatial Variation**: Noise values are determined by world position, creating consistent spatial patterns.

### Behavior

**Noise Filter Evaluation:**

```
Noise at point positions (Perlin, Scale 100):
   Point A at (100, 200, 0): Noise = 0.73
   Point B at (150, 250, 0): Noise = 0.45
   Point C at (200, 300, 0): Noise = 0.82

Comparison: Noise >= 0.5

Results:
   Point A: 0.73 >= 0.5 → PASS
   Point B: 0.45 >= 0.5 → FAIL
   Point C: 0.82 >= 0.5 → PASS
```

### Inputs

| Pin        | Type   | Description                                            |
| ---------- | ------ | ------------------------------------------------------ |
| **Noises** | Params | Noise generator sub-nodes defining the noise to sample |

### Settings

#### Comparison Settings

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator used to test noise against the threshold.

| Option               | Description                            |
| -------------------- | -------------------------------------- |
| **Equal**            | Noise == Threshold                     |
| **Not Equal**        | Noise != Threshold                     |
| **Greater**          | Noise > Threshold                      |
| **Greater or Equal** | Noise >= Threshold                     |
| **Less**             | Noise < Threshold                      |
| **Less or Equal**    | Noise <= Threshold                     |
| **Nearly Equal**     | Noise ≈ Threshold (within tolerance)   |
| **Nearly Not Equal** | Noise !≈ Threshold (outside tolerance) |

Default: `Nearly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the threshold value comes from a constant or an attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use a fixed threshold value           |
| **Attribute** | Read threshold from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Constant</strong> <code>double</code></summary>

The constant threshold value to compare against.

Default: `0.5`

📋 _Visible when Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read threshold values from.

Default: `OperandB`

📋 _Visible when Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for Nearly Equal/Not Equal comparisons.

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison = Nearly Equal or Nearly Not Equal_

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Filter Settings for common filter settings (Invert, priority, etc.).

### Outputs

| Pin        | Type   | Description                                             |
| ---------- | ------ | ------------------------------------------------------- |
| **Filter** | Params | Filter factory for connection to filter-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Filters/Points/PCGExNoiseFilter.h)
