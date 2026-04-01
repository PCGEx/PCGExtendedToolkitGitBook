---
description: Compares attribute values between datasets.
icon: circle-dashed
---

# Match : Attributes

### Overview

This match rule establishes correspondences between data elements by comparing attribute values. It reads an attribute from candidate data (typically from the @Data domain) and compares it against an attribute on target elements (either point or data domain). The comparison can be numeric (with various operators like equal, greater than, etc.) or string-based (exact match, contains, etc.), making it versatile for both quantitative and categorical matching scenarios.

### How It Works

1. **Candidate Attribute Read**: Reads the specified attribute from the candidate dataset's @Data domain
2. **Target Attribute Read**: Reads the specified attribute from target elements (point or data domain depending on context)
3. **Type Detection**: Determines whether to perform numeric or string comparison based on the Check setting
4. **Comparison Execution**: Evaluates the comparison using the selected operator:
   * **Numeric**: Supports equality, inequality, greater/less than, nearly equal (with tolerance)
   * **String**: Supports exact matching, contains, starts/ends with, pattern matching
5. **Operand Swap**: Optionally swaps operands (candidate vs target) for the comparison
6. **Strictness Application**: Applies the inherited Strictness setting if multiple values are involved
7. **Inversion**: Applies the inherited Invert flag if enabled

### Behavior

**Numeric Comparison Examples:**

```
Candidate Key = 5, Target Value = 5
StrictlyEqual → Match ✓

Candidate Key = 5.001, Target Value = 5.0
NearlyEqual (Tolerance=0.01) → Match ✓
StrictlyEqual → No Match ✗

Candidate Key = 10, Target Value = 5
GreaterThan → Match ✓
```

**String Comparison Examples:**

```
Candidate Name = "Apple", Target Name = "Apple"
StrictlyEqual → Match ✓

Candidate Name = "Apple", Target Name = "apple"
StrictlyEqual → No Match ✗ (case-sensitive)

Candidate Tag = "fruit", Target Tags = "fruit,vegetable"
Contains → Match ✓
```

**Operand Swap:**

```
Normal: Candidate.Key > Target.Value
Swapped: Target.Value > Candidate.Key
(Useful for reversing comparison direction)
```

Good for: key-based matching, ID correspondence, category filtering, value-based pairing, database-style joins

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Candidate Attribute Name</strong> <code>FName</code></summary>

The attribute to read from candidate datasets. Only supports @Data domain and will only attempt to read from there.

This is the attribute on the "matching" side of the operation - the dataset being tested for matches.

Examples:

* `"Key"` (default): Reads a "Key" attribute
* `"ID"`: Reads an "ID" attribute
* `"Category"`: Reads a category label

Default: `"Key"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target Attribute Name</strong> <code>FName</code></summary>

The attribute to read from target elements. Depending on where the match operates, this can be read from a target point or data domain. If only data domain is supported, will read the first element value.

This is the attribute on the "target" side - the dataset being matched against.

Examples:

* `"@Data.Value"` (default): Reads "Value" from data domain
* `"PointID"`: Reads from point attribute
* `"@Data.Category"`: Reads "Category" from data domain

Default: `"@Data.Value"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Check</strong> <code>EPCGExComparisonDataType</code></summary>

Determines how the data should be compared.

| Option                | Description                                                                 |
| --------------------- | --------------------------------------------------------------------------- |
| **Numeric** (default) | Compare as numbers (int, float, double). Uses numeric comparison operators. |
| **String**            | Compare as text strings. Uses string comparison operators.                  |

The attribute values will be cast to the selected type for comparison.

Default: `Numeric`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Numeric Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when Check is set to Numeric.

| Operator                     | Description                              |
| ---------------------------- | ---------------------------------------- |
| **Strictly Equal** (default) | Candidate == Target (exact match)        |
| **Strictly Not Equal**       | Candidate != Target                      |
| **Nearly Equal**             | Candidate ≈ Target (within Tolerance)    |
| **Nearly Not Equal**         | Candidate not within Tolerance of Target |
| **Greater Than**             | Candidate > Target                       |
| **Greater Or Equal**         | Candidate >= Target                      |
| **Less Than**                | Candidate < Target                       |
| **Less Or Equal**            | Candidate <= Target                      |

Default: `Strictly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Rounding tolerance for "Nearly Equal" and "Nearly Not Equal" comparisons. Values within this distance are considered equal.

Only used when Numeric Comparison is set to Nearly Equal or Nearly Not Equal.

* **Small (1e-6)**: Very precise matching
* **Default (DBL\_COMPARE\_TOLERANCE)**: Standard floating-point tolerance
* **Large (0.01)**: Loose matching

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

</details>

<details>

<summary><strong>String Comparison</strong> <code>EPCGExStringComparison</code></summary>

The comparison operator to use when Check is set to String.

| Operator                     | Description                                 |
| ---------------------------- | ------------------------------------------- |
| **Strictly Equal** (default) | Exact string match (case-sensitive)         |
| **Strictly Not Equal**       | Strings are different                       |
| **Contains**                 | Target contains Candidate substring         |
| **Starts With**              | Target starts with Candidate                |
| **Ends With**                | Target ends with Candidate                  |
| **Matches Pattern**          | Target matches regex pattern from Candidate |

Default: `Strictly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Swap Operands</strong> <code>bool</code></summary>

When enabled, swaps the operands during comparison.

* **false** (default): Normal order (Candidate operator Target)
* **true**: Swapped order (Target operator Candidate)

Example:

* Normal: Candidate.Value > Target.Value
* Swapped: Target.Value > Candidate.Value

Useful for reversing comparison direction without changing the operator.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

### Outputs

| Pin            | Type       | Description                                     |
| -------------- | ---------- | ----------------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory for attribute-based matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchAttrToAttr.h)
