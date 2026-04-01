---
description: Compares a numeric attribute value read from the nearest target point.
icon: circle-dashed
---

# Filter : Compare Nearest (Numeric)

### Overview

This filter evaluates points by finding the nearest target point and reading a numeric attribute from it, then comparing that value against a threshold. Unlike the standard numeric compare filter which reads both operands from the same point, this filter reads Operand A from the closest target point based on distance. This enables proximity-based attribute comparisons across separate point collections.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Target Resolution**: Finds the nearest target point for each input point using the configured distance method.
2. **Value Reading**: Reads Operand A from the nearest target point's attribute.
3. **Comparison**: Compares the target's value against Operand B using the selected operator.
4. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Targets Input**: Requires a "Targets" input pin providing the reference point collection.
* **Cross-Collection**: Operand A is read from the target, Operand B from the source or a constant.
* **Self-Exclusion**: Can exclude a collection from being matched against itself.

### Behavior

**Nearest Comparison:**

```
Source Points:  [P1 at (0,0,0), P2 at (100,0,0)]
Target Points:  [T1 at (10,0,0) Score=80, T2 at (90,0,0) Score=40]
Operand A: $Score (from target)
Operand B: 50 (constant)
Comparison: >=

P1 → Nearest target: T1 (Score=80) → 80 >= 50: Pass
P2 → Nearest target: T2 (Score=40) → 40 >= 50: Fail
```

### Inputs

| Pin         | Type   | Description                                               |
| ----------- | ------ | --------------------------------------------------------- |
| **Targets** | Points | Target points to find the nearest from and read Operand A |

### Settings

<details>

<summary><strong>Distance Details</strong> <code>FPCGExDistanceDetails</code></summary>

Configuration for how distances are measured between source and target points. Includes options for center-to-center, bounds-based, and other distance calculation methods.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read from the nearest target point. Value is converted to double.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use.

| Option   | Description                          |
| -------- | ------------------------------------ |
| **==**   | Strictly equal                       |
| **!=**   | Strictly not equal                   |
| **>=**   | Equal or greater                     |
| **<=**   | Equal or smaller                     |
| **>**    | Strictly greater                     |
| **<**    | Strictly smaller                     |
| **\~=**  | Nearly equal (within tolerance)      |
| **!\~=** | Nearly not equal (outside tolerance) |

Default: `Nearly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant value or read from an attribute for Operand B.

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use the specified constant value |
| **Attribute** | Read from a point attribute      |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read as Operand B. Value is converted to double.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>double</code></summary>

The constant value to compare against.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for the comparison.

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

📋 _Visible when Comparison is Nearly Equal or Nearly Not Equal_

</details>

<details>

<summary><strong>Ignore Self</strong> <code>bool</code></summary>

When enabled, excludes the point's own data collection from the nearest search.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy, Missing Data Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExNumericCompareNearestFilter.h)
