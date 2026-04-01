---
description: Filter using bitflag comparison.
icon: circle-dashed
---

# Filter : Bitmask

### Overview

This filter evaluates points by performing bitwise comparisons between a flag attribute and a bitmask value. It supports various comparison modes including partial matches, exact matches, and no-match conditions. The bitmask can be a constant value or read from another attribute, and additional bitmask compositions can be applied.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Flag Reading**: Reads the int64 flags attribute from each point.
2. **Mask Composition**: Applies any configured bitmask compositions to the comparison mask.
3. **Bitwise Comparison**: Performs the selected comparison operation between flags and mask.
4. **Result**: Returns pass or fail based on the comparison outcome.

**Usage Notes**

* **Int64 Requirement**: Both flags and mask attributes must be int64 type.
* **Compositions**: External bitmask compositions are OR'd into the final mask before comparison.
* **State Integration**: Works well with State nodes that write flag values to attributes.

### Behavior

**Bitmask Comparison Modes:**

```
Flags:  0b00110101  (decimal: 53)
Mask:   0b00000101  (decimal: 5)

Match Partial:    Pass if (Flags & Mask) != 0
                  (53 & 5) = 5 != 0 → Pass (some bits match)

Match All:        Pass if (Flags & Mask) == Mask
                  (53 & 5) = 5 == 5 → Pass (all mask bits present)

Match Exact:      Pass if Flags == Mask
                  53 != 5 → Fail (not identical)

No Match:         Pass if (Flags & Mask) == 0
                  (53 & 5) = 5 != 0 → Fail (bits overlap)
```

**Visual Example:**

```
Flags:  0 0 1 1 0 1 0 1
Mask:   0 0 0 0 0 1 0 1
AND:    0 0 0 0 0 1 0 1  ← Result has bits set

Match Partial: Pass (result != 0)
Match All:     Pass (result == mask)
No Match:      Fail (result != 0)
```

### Settings

<details>

<summary><strong>Flags Attribute</strong> <code>FName</code></summary>

The int64 attribute containing the flags to test (Operand A).

Default: `Flags`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExBitflagComparison</code></summary>

The type of bitwise comparison to perform.

| Option            | Description                                       |
| ----------------- | ------------------------------------------------- |
| **Match Partial** | Pass if any bits in the mask are set in the flags |
| **Match All**     | Pass if all bits in the mask are set in the flags |
| **Match Exact**   | Pass only if flags exactly equal the mask         |
| **No Match**      | Pass if no bits in the mask are set in the flags  |

Default: `Match Partial`

</details>

<details>

<summary><strong>Mask Input</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant bitmask or read from an attribute.

| Option        | Description                             |
| ------------- | --------------------------------------- |
| **Constant**  | Use the specified constant bitmask      |
| **Attribute** | Read the bitmask from a point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Bitmask (Attr)</strong> <code>FName</code></summary>

The int64 attribute containing the comparison mask (Operand B).

Default: `Mask`

⚡ PCG Overridable

📋 _Visible when Mask Input = Attribute_

</details>

<details>

<summary><strong>Bitmask</strong> <code>int64</code></summary>

The constant bitmask value to compare against (Operand B).

Default: `0`

⚡ PCG Overridable

📋 _Visible when Mask Input = Constant_

</details>

<details>

<summary><strong>Compositions</strong> <code>TArray&#x3C;FPCGExBitmaskRef></code></summary>

External bitmask compositions to apply to Operand B. These are OR'd together with the base bitmask before comparison.

</details>

<details>

<summary><strong>Invert Result</strong> <code>bool</code></summary>

Invert the filter result.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExBitmaskFilter.h)
