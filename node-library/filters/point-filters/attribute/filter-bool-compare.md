---
description: (bool) A == (bool) B
icon: circle-dashed
---

# Filter : Bool Compare

### Overview

This filter compares boolean attribute values at each point. It reads a boolean attribute (Operand A) and compares it against either a constant boolean value or another boolean attribute (Operand B). The filter supports both equality and inequality comparisons.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads the boolean value from Operand A attribute for each point.
2. **Comparison Value**: Gets the comparison value from either a constant or Operand B attribute.
3. **Boolean Comparison**: Performs equality or inequality test between the two values.
4. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Type Conversion**: Non-boolean attributes are converted to bool (0 = false, non-zero = true).
* **Simple Logic**: For straightforward boolean attribute filtering without complex conditions.

### Behavior

**Boolean Comparison:**

```
Point A: Operand A = true,  Operand B = true   → Equal: Pass, Not Equal: Fail
Point B: Operand A = true,  Operand B = false  → Equal: Fail, Not Equal: Pass
Point C: Operand A = false, Operand B = false  → Equal: Pass, Not Equal: Fail
Point D: Operand A = false, Operand B = true   → Equal: Fail, Not Equal: Pass
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read as Operand A. Value is converted to boolean.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExEquality</code></summary>

The comparison operator to use.

| Option        | Description                |
| ------------- | -------------------------- |
| **Equal**     | Pass if A equals B         |
| **Not Equal** | Pass if A does not equal B |

Default: `Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant value or read from an attribute for Operand B.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use the specified constant boolean |
| **Attribute** | Read from a point attribute        |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read as Operand B. Value is converted to boolean.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>bool</code></summary>

The constant boolean value to compare against.

Default: `true`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExBooleanCompareFilter.h)
