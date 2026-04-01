---
description: Test the value of one or multiple tags on the input collection.
icon: circle-dashed
---

# Data Filter : Tag Value

### Overview

This filter evaluates the value portion of tags formatted as `Tag:Value` on data collections. It supports both numeric and string value comparisons, making it useful for filtering based on tag metadata. The filter can match multiple tags and combine results using AND or OR logic.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Tag Matching**: Finds tags matching the specified name pattern.
2. **Value Extraction**: Extracts the value portion from matched `Tag:Value` formatted tags.
3. **Type-Specific Comparison**: Compares values using numeric or string comparison depending on configuration.
4. **Multi-Match Logic**: When multiple tags match, combines results using AND or OR logic.
5. **Result**: Returns pass if the comparison(s) succeed, fail otherwise.

**Usage Notes**

* **Collection Filter**: This filter evaluates at the data set level, not per-point.
* **Value Format**: Tags must be in `Tag:Value` format; tags without values will not match.
* **Type Strictness**: The value type check is strict; numeric mode expects parseable numbers.

### Behavior

**Tag Value Check Examples:**

```
Collection tags: ["Priority:5", "Category:Enemy", "Level:3"]

Config: Tag="Priority", ValueType=Numeric, Comparison=GreaterThan, OperandB=3
Result: Pass (5 > 3)

Config: Tag="Category", ValueType=String, Comparison=Contains, OperandB="Enem"
Result: Pass ("Enemy" contains "Enem")

Config: Tag="Level", ValueType=Numeric, Comparison=Equals, OperandB=5
Result: Fail (3 != 5)
```

**Multi-Match Example:**

```
Collection tags: ["Score:10", "Score:20", "Score:30"]

Config: Tag="Score", ValueType=Numeric, Comparison=GreaterThan, OperandB=15, MultiMatch=OR
Result: Pass (20 > 15 or 30 > 15)

Config: Tag="Score", ValueType=Numeric, Comparison=GreaterThan, OperandB=15, MultiMatch=AND
Result: Fail (10 > 15 is false)
```

### Settings

<details>

<summary><strong>Tag Name</strong> <code>FString</code></summary>

The tag name to search for. Can match multiple tags depending on the Match mode.

Default: `Tag`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match</strong> <code>EPCGExStringMatchMode</code></summary>

How to match the tag name.

| Option         | Description                            |
| -------------- | -------------------------------------- |
| **Equals**     | Exact match required                   |
| **Contains**   | Tag name contains the search string    |
| **StartsWith** | Tag name starts with the search string |
| **EndsWith**   | Tag name ends with the search string   |

Default: `Equals`

</details>

<details>

<summary><strong>Value Type</strong> <code>EPCGExComparisonDataType</code></summary>

The expected type of the tag value. This is a strict check.

| Option      | Description                   |
| ----------- | ----------------------------- |
| **Numeric** | Parse and compare as a number |
| **String**  | Compare as a string           |

Default: `Numeric`

</details>

#### Numeric Comparison Settings

<details>

<summary><strong>Comparison (Numeric)</strong> <code>EPCGExComparison</code></summary>

The comparison operator for numeric values.

Default: `Nearly Equal`

📋 _Visible when Value Type = Numeric_

</details>

<details>

<summary><strong>Operand B (Numeric)</strong> <code>double</code></summary>

The numeric value to compare against.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Value Type = Numeric_

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for numeric comparisons.

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

📋 _Visible when Value Type = Numeric and Comparison is Nearly Equal/Not Equal_

</details>

#### String Comparison Settings

<details>

<summary><strong>Comparison (String)</strong> <code>EPCGExStringComparison</code></summary>

The comparison operator for string values.

Default: `Contains`

📋 _Visible when Value Type = String_

</details>

<details>

<summary><strong>Operand B (String)</strong> <code>FString</code></summary>

The string value to compare against.

Default: `Tag`

⚡ PCG Overridable

📋 _Visible when Value Type = String_

</details>

#### Result Settings

<details>

<summary><strong>Multi Match</strong> <code>EPCGExFilterGroupMode</code></summary>

How to combine results when multiple tags match the name pattern.

| Option  | Description                                |
| ------- | ------------------------------------------ |
| **AND** | All matching tags must pass the comparison |
| **OR**  | At least one matching tag must pass        |

Default: `AND`

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the result of this filter.

Default: `false`

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                         | Description                   |
| ---------- | ---------------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Collection) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Collections/PCGExTagValueFilter.h)
