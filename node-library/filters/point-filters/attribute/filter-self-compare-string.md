---
description: Compares a string attribute value against itself at another index.
icon: circle-dashed
---

# Filter : Self Compare (String)

### Overview

This filter compares a string attribute at each point against the same attribute at a different point within the same collection. The target index can be specified as an absolute pick or as a relative offset from the current point. All string comparison modes are available, including equality, length checks, lexicographic ordering, and substring matching. This enables sequence-based string filtering such as detecting duplicate labels, finding where names change, or checking substring relationships between neighbors.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads the string from the specified attribute at the current point.
2. **Index Resolution**: Computes the comparison index using Pick (absolute) or Offset (relative) mode.
3. **Self Comparison**: Reads the same attribute at the resolved index and compares the two string values.
4. **Result**: Returns pass if the comparison succeeds, or the fallback value for invalid indices.

**Usage Notes**

* **Offset Mode**: An offset of -1 compares each point's string against the previous point's string.
* **Swap Operands**: For directional comparisons like Contains, swapping operands changes whether you're testing "current contains neighbor" or "neighbor contains current."
* **Ordered Data**: Most useful with ordered point collections where index relationships are meaningful.

### Behavior

**Self Comparison (Offset Mode):**

```
Attribute: $Label
Index Offset: -1 (previous point)
Comparison: ==

Point 0: Label="A", Previous=N/A  → Invalid index → Fallback: Fail
Point 1: Label="A", Previous="A"  → "A" == "A": Pass (same)
Point 2: Label="B", Previous="A"  → "B" == "A": Fail (changed)
Point 3: Label="B", Previous="B"  → "B" == "B": Pass (same)
```

**Contains with Swap:**

```
Attribute: $Name
Index Offset: 1 (next point)
Comparison: Contains
Swap Operands: true

Point 0: "Tree", Next="TreeStump" → "TreeStump" contains "Tree": Pass
Point 1: "TreeStump", Next="Rock" → "Rock" contains "TreeStump": Fail
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>FName</code></summary>

The attribute name to read and compare against itself at a different index.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExStringComparison</code></summary>

The string comparison mode to use.

| Option                    | Description                                     |
| ------------------------- | ----------------------------------------------- |
| **==**                    | Strictly equal                                  |
| **!=**                    | Strictly not equal                              |
| **LengthStrictlyEqual**   | String lengths are equal                        |
| **LengthStrictlyUnequal** | String lengths are not equal                    |
| **LengthEqualOrGreater**  | A's length is equal to or greater than B's      |
| **LengthEqualOrSmaller**  | A's length is equal to or smaller than B's      |
| **StrictlyGreater**       | A is lexicographically greater than B           |
| **StrictlySmaller**       | A is lexicographically smaller than B           |
| **LocaleStrictlyGreater** | A is greater than B using locale-aware ordering |
| **LocaleStrictlySmaller** | A is smaller than B using locale-aware ordering |
| **Contains**              | A contains B as a substring                     |
| **Starts With**           | A starts with B                                 |
| **Ends With**             | A ends with B                                   |

Default: `Strictly Equal`

⚡ PCG Overridable

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

The constant index value. In Offset mode, -1 means previous point, 1 means next point.

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

<details>

<summary><strong>Swap Operands</strong> <code>bool</code></summary>

Swaps the order of operands for the comparison. Useful to invert directional checks like Contains, Starts With, and Ends With.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExStringSelfCompareFilter.h)
