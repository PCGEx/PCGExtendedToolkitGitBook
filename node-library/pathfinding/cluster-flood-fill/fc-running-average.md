---
description: >-
  Ignore candidates whose attribute value isn't within the given tolerance of a
  running average.
icon: circle-dashed
---

# FC : Running Average

### Overview

This fill control sub-node maintains a running average of an attribute value along the diffusion path and rejects candidates that deviate too far from this average. This creates paths that maintain consistent attribute values, useful for following contours or avoiding sudden changes in properties like height, density, or other numeric attributes.

### How It Works

1. **Track Values**: Maintains a windowed history of attribute values along the current path.
2. **Compute Average**: Calculates the running average over the window of recent values.
3. **Check Tolerance**: For each candidate, compares its attribute value against the running average.
4. **Reject Outliers**: If the candidate's value differs from the average by more than the tolerance, it's rejected.

**Usage Notes**

* **Contour Following**: Useful for following terrain contours by averaging height values and rejecting vertices that climb or descend too steeply.
* **Window Size**: Larger windows smooth out variations over more samples. Smaller windows react more quickly to changes.
* **Default Operand**: By default, uses `$Position.Z` (height), but can be any numeric attribute.

### Behavior

```
Running Average (Tolerance = 5, Window = 3):

Path with Z values:
  Seed(10) → [12] → [11] → [9] → [25] → STOP
              ↓       ↓      ↓      ↓
  Avg:       10      11    11.3   10.7
  Delta:      2       0    -2.3   14.3 (exceeds tolerance 5)

The path follows gradual changes but stops at sudden jumps.
```

### Settings

<details>

<summary><strong>Window Size Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the window size comes from a constant or attribute.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use a fixed window size            |
| **Attribute** | Read window size from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Window Size (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the window size from.

Default: `WindowSize`

📋 _Visible when Window Size Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Window Size</strong> <code>int32</code></summary>

Number of recent values to include in the running average calculation.

Default: `10`

Minimum: 1

📋 _Visible when Window Size Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the tolerance comes from a constant or attribute.

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use a fixed tolerance value      |
| **Attribute** | Read tolerance from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Tolerance (Attr)</strong> <code>FName</code></summary>

Attribute name to read the tolerance from.

Default: `Tolerance`

📋 _Visible when Tolerance Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Maximum allowed deviation from the running average. Candidates with values differing by more than this are rejected.

Default: `10`

Minimum: 0

📋 _Visible when Tolerance Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute or property to track for averaging. The value is broadcast to `double` for comparison.

Default: `$Position.Z`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source

Note: The Steps setting is not available for this control.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlRunningAverage.h)
