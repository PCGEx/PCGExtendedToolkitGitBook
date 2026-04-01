---
description: A % B != C
icon: circle-dashed
---

# Filter : Modulo Compare

### Overview

This filter evaluates points by computing the modulo (remainder) of one value divided by another, then comparing the result against a third value. This enables pattern-based filtering such as selecting every Nth point, detecting even/odd indices, or creating repeating selection patterns across a dataset.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads Operand A from the specified attribute for each point.
2. **Modulo Calculation**: Computes `A % B` (remainder of A divided by B).
3. **Comparison**: Compares the modulo result against Operand C using the selected operator.
4. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Zero Handling**: When Operand B is zero (division by zero), returns the configured Zero Result value.
* **Double Conversion**: All values are internally converted to double for computation.
* **Pattern Selection**: Setting B=3, C=0 with Equal selects every 3rd point (where index % 3 == 0).

### Behavior

**Modulo Comparison:**

```
Operand A: Point index attribute
Operand B: 3 (modulo base)
Operand C: 0 (comparison value)
Comparison: NearlyEqual

Point 0: 0 % 3 = 0  → 0 ~= 0: Pass
Point 1: 1 % 3 = 1  → 1 ~= 0: Fail
Point 2: 2 % 3 = 2  → 2 ~= 0: Fail
Point 3: 3 % 3 = 0  → 0 ~= 0: Pass
Point 4: 4 % 3 = 1  → 1 ~= 0: Fail
Point 5: 5 % 3 = 2  → 2 ~= 0: Fail
Point 6: 6 % 3 = 0  → 0 ~= 0: Pass
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read as Operand A. Value is converted to double.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B Source</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant value or read from an attribute for Operand B (the modulo base).

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use the specified constant value |
| **Attribute** | Read from a point attribute      |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read as Operand B (modulo base). Value is converted to double.

⚡ PCG Overridable

📋 _Visible when Operand B Source = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>double</code></summary>

The constant modulo base value.

Default: `2`

⚡ PCG Overridable

📋 _Visible when Operand B Source = Constant_

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when testing the modulo result against Operand C.

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

Whether to use a constant value or read from an attribute for Operand C (the comparison target).

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use the specified constant value |
| **Attribute** | Read from a point attribute      |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand C (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read as Operand C. Value is converted to double.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand C</strong> <code>double</code></summary>

The constant value to compare the modulo result against.

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

<summary><strong>Zero Result</strong> <code>bool</code></summary>

The value to return when Operand B is zero (division by zero scenario).

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExModuloCompareFilter.h)
