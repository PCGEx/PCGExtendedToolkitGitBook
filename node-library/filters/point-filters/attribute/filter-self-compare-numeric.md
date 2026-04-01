---
description: >-
  Compares an attribute value at the current point against the same attribute at
  a different index.
icon: circle-dashed
---

# Filter : Self Compare (Numeric)

### Overview

This filter compares a numeric attribute value at each point against the same attribute at another point within the same collection. The target index can be specified as an absolute pick or as a relative offset from the current point. This enables sequence-based filtering such as detecting increasing/decreasing values, finding duplicates, or comparing neighbors in ordered point sets.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads Operand A from the specified attribute at the current point index.
2. **Index Resolution**: Computes the comparison index using Pick (absolute) or Offset (relative) mode.
3. **Self Comparison**: Reads the same attribute at the resolved index and compares the two values.
4. **Result**: Returns pass if the comparison succeeds, or the fallback value for invalid indices.

**Usage Notes**

* **Index Safety**: Controls how out-of-range indices are handled (clamped, tiled, etc.).
* **Offset Mode**: An offset of -1 compares each point against the previous point.
* **Ordered Data**: Most useful with ordered point collections where index relationships are meaningful.

### Behavior

**Self Comparison (Offset Mode):**

```
Attribute: $Height
Index Offset: -1 (previous point)
Comparison: >

Point 0: Height=10, Previous=N/A  → Invalid index → Fallback: Fail
Point 1: Height=20, Previous=10   → 20 > 10: Pass (increasing)
Point 2: Height=15, Previous=20   → 15 > 20: Fail (decreasing)
Point 3: Height=25, Previous=15   → 25 > 15: Pass (increasing)
```

**Pick Mode:**

```
Attribute: $Score
Index Pick: 0 (first point)
Comparison: >=

Compare every point's score against the first point's score.
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read and compare against itself. Value is converted to double.

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

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for the comparison.

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

📋 _Visible when Comparison is Nearly Equal or Nearly Not Equal_

</details>

<details>

<summary><strong>Index Mode</strong> <code>EPCGExIndexMode</code></summary>

How to interpret the index value.

| Option     | Description                                           |
| ---------- | ----------------------------------------------------- |
| **Pick**   | Use as an absolute index into the collection          |
| **Offset** | Use as a relative offset from the current point index |

Default: `Offset`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant index or read from an attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use the specified constant index      |
| **Attribute** | Read the index from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read the index from. Value is converted to int32.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Index</strong> <code>int32</code></summary>

The constant index value.

Default: `-1`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle out-of-range indices.

| Option     | Description                       |
| ---------- | --------------------------------- |
| **Ignore** | Skip the test for invalid indices |
| **Tile**   | Wrap around using modulo          |
| **Clamp**  | Clamp to valid range              |
| **Yoyo**   | Bounce back and forth             |

Default: `Clamp`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invalid Index Fallback</strong> <code>EPCGExFilterFallback</code></summary>

Result to return when the resolved index is invalid.

| Option   | Description                     |
| -------- | ------------------------------- |
| **Pass** | Return pass for invalid indices |
| **Fail** | Return fail for invalid indices |

Default: `Fail`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExNumericSelfCompareFilter.h)
