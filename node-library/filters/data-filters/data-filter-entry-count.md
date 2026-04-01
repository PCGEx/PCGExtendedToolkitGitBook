---
description: Does a numeric comparison against the number of entries in a collection.
icon: circle-dashed
---

# Data Filter : Entry Count

### Overview

This filter evaluates the point count of a data collection against a comparison value. It operates at the collection level, passing or failing entire point sets based on whether their entry count satisfies the specified condition. Useful for filtering out empty collections, ensuring minimum data requirements, or segregating data by size.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Count Retrieval**: Gets the number of points/entries in the data collection.
2. **Comparison**: Compares the count against the operand using the selected comparison operator.
3. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Collection Filter**: This filter evaluates at the data set level, not per-point.
* **Empty Check**: Use `> 0` to filter out empty collections.
* **Size Ranges**: Combine with other filters or use Nearly Equal with tolerance for range checks.

### Behavior

**Entry Count Check Examples:**

```
Collection A: 100 points
Collection B: 0 points
Collection C: 50 points

Config: Comparison = GreaterThan, OperandB = 0
Results: A=Pass, B=Fail, C=Pass

Config: Comparison = NearlyEqual, OperandB = 50, Tolerance = 10
Results: A=Fail, B=Fail, C=Pass (50 is within 10 of 50)
```

### Settings

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when testing the entry count.

| Option                    | Description                                |
| ------------------------- | ------------------------------------------ |
| **Equal**                 | Count must exactly equal operand           |
| **Not Equal**             | Count must not equal operand               |
| **Greater Than**          | Count must be greater than operand         |
| **Greater Than Or Equal** | Count must be at least operand             |
| **Less Than**             | Count must be less than operand            |
| **Less Than Or Equal**    | Count must be at most operand              |
| **Nearly Equal**          | Count must be within tolerance of operand  |
| **Nearly Not Equal**      | Count must be outside tolerance of operand |

Default: `Nearly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant value or read from an attribute.

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use the specified constant value |
| **Attribute** | Read from a data attribute       |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the comparison value from. Value will be converted to int32.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>int32</code></summary>

The constant value to compare the entry count against.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

The tolerance range for near-equality comparisons.

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

📋 _Visible when Comparison is Nearly Equal or Nearly Not Equal_

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                         | Description                   |
| ---------- | ---------------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Collection) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Collections/PCGExEntryCountFilter.h)
