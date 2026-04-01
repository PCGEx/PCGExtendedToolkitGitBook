---
description: Compares two string attribute values.
icon: circle-dashed
---

# Filter : Compare (String)

### Overview

This filter compares a string attribute (Operand A) against another string value using various comparison modes. Operand B can be a constant string or read from another attribute. Comparison modes include exact equality, length comparisons, lexicographic ordering, and substring checks (contains, starts with, ends with). Operand order can be swapped to invert directional checks like "contains."

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Read Operand A**: Reads the string value from the specified attribute on each point.
2. **Read Operand B**: Reads the comparison value from a constant or another attribute.
3. **Compare**: Applies the selected string comparison between A and B.
4. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Swap Operands**: For directional comparisons like Contains, swapping operands changes whether you're testing "A contains B" or "B contains A."
* **Attribute Names**: Operand A is specified as an attribute name (`FName`), not a selector. The attribute value is read as a string.

### Behavior

**Equality Check:**

```
Operand A: $Tag
Operand B: "Enemy"
Comparison: ==

Point 0: Tag="Enemy"  → "Enemy" == "Enemy": Pass
Point 1: Tag="Ally"   → "Ally"  == "Enemy": Fail
Point 2: Tag="Enemy"  → "Enemy" == "Enemy": Pass
```

**Contains Check:**

```
Operand A: $Name
Operand B: "Tree"
Comparison: Contains

Point 0: Name="OakTree"    → "OakTree" contains "Tree": Pass
Point 1: Name="Rock"       → "Rock" contains "Tree": Fail
Point 2: Name="TreeStump"  → "TreeStump" contains "Tree": Pass

With Swap Operands = true:
Point 0: Name="Oak" → "Tree" contains "Oak": Fail
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>FName</code></summary>

The attribute name to read the first string value from.

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

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant string or read Operand B from an attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use the specified constant string     |
| **Attribute** | Read the value from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FName</code></summary>

The attribute name to read the second string value from.

Default: `None`

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>FString</code></summary>

The constant string to compare against.

Default: `MyString`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExStringCompareFilter.h)
