---
description: Compares tags on candidates against attribute values on targets.
icon: circle-dashed
---

# Match : Tags × Attributes

### Overview

This match rule establishes correspondences by comparing tag values from candidate data against attribute values from target data. It bridges the gap between tag-based metadata (on candidates) and attribute-based data (on targets), enabling matches based on whether a candidate's tag matches a target's attribute value. The rule supports both name-only matching and combined name+value matching, with flexible comparison operators for both numeric and string data.

### How It Works

1. **Tag Name Determination**: Gets the tag name from candidate:
   * **Constant**: Fixed tag name for all candidates
   * **Attribute**: Per-candidate tag name from attribute
2. **Tag Retrieval**: Looks up the tag on the candidate data
3. **Name Matching**: If NameMatch is not Equals, tests if tag name matches pattern
4. **Attribute Read**: Reads the ValueAttribute from the target
5. **Value Comparison**: If bDoValueMatch is enabled:
   * Compares tag value against attribute value
   * Uses numeric or string comparison based on ValueType
6. **Match Result**: Returns true if tag exists and (if enabled) value matches
7. **Strictness Application**: Applies inherited Strictness setting
8. **Inversion**: Applies inherited Invert flag if enabled

### Behavior

**Basic Tag-to-Attribute Match:**

```
Candidate has Tag "Category" = "Fruit"
Target has Attribute "Type" = "Fruit"
→ Match ✓ (tag value equals attribute value)

Candidate has Tag "Category" = "Vegetable"
Target has Attribute "Type" = "Fruit"
→ No Match ✗ (values don't match)
```

**Name-Only Match (bDoValueMatch = false):**

```
Candidate has Tag "Category" (any value)
→ Match ✓ (tag exists, value not checked)

Candidate doesn't have Tag "Category"
→ No Match ✗ (tag doesn't exist)
```

**Numeric Comparison:**

```
Candidate Tag "Score" = "85"
Target Attribute "Threshold" = 80
Comparison = GreaterThan
→ Match ✓ (85 > 80)
```

**String Comparison:**

```
Candidate Tag "Region" = "North_America"
Target Attribute "Filter" = "North"
Comparison = StartsWith
→ Match ✓ ("North_America" starts with "North")
```

**Name Match Modes:**

```
Equals: Tag name must exactly match TagName
Contains: Tag name contains TagName substring
StartsWith/EndsWith: Tag name position matching
```

**Attribute-Based Tag Name:**

```
Candidate[0] has TagNameAttr = "Priority"
  Looks for Tag "Priority" on Candidate[0]
Candidate[1] has TagNameAttr = "Status"
  Looks for Tag "Status" on Candidate[1]
```

Good for: tag-to-data bridging, metadata validation, hybrid matching, category filtering, cross-domain correspondence

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Tag Name Input</strong> <code>EPCGExInputValueType</code></summary>

Determines where the tag name comes from.

| Option                 | Description                                                          |
| ---------------------- | -------------------------------------------------------------------- |
| **Constant** (default) | Uses the fixed TagName string for all candidates                     |
| **Attribute**          | Reads tag name from TagNameAttribute for per-candidate tag selection |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Name (Attribute)</strong> <code>FName</code></summary>

Attribute to read the tag name from when TagNameInput is Attribute.

Examples:

* `"ReadTagFrom"` (default): Reads which tag to check from this attribute
* `"CategoryTag"`: Each candidate specifies its category tag
* `"MetadataKey"`: Dynamic tag selection per element

Only used when TagNameInput = Attribute.

Default: `"ReadTagFrom"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Name (Constant)</strong> <code>FString</code></summary>

Fixed tag name to look for when TagNameInput is Constant.

Examples:

* `"TagOnInput"` (default): Looks for tag named "TagOnInput"
* `"Category"`: Looks for tag named "Category"
* `"Type"`: Looks for tag named "Type"

Only used when TagNameInput = Constant.

Default: `"TagOnInput"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Name Match</strong> <code>EPCGExStringMatchMode</code></summary>

How to match the tag name against the specified TagName.

| Option               | Description                            |
| -------------------- | -------------------------------------- |
| **Equals** (default) | Tag name must exactly match TagName    |
| **Contains**         | Tag name contains TagName as substring |
| **Starts With**      | Tag name starts with TagName           |
| **Ends With**        | Tag name ends with TagName             |

Allows flexible tag name matching beyond exact equality.

Default: `Equals`

</details>

<details>

<summary><strong>Do Value Match</strong> <code>bool</code></summary>

When enabled, compares the tag's value against the target's attribute value. When disabled, only checks if the tag exists.

* **false** (default): Only verify tag existence
* **true**: Match tag value against attribute value

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Value Type</strong> <code>EPCGExComparisonDataType</code></summary>

Expected value type for comparison. This is a strict check - values are cast to this type.

| Option                | Description                                    |
| --------------------- | ---------------------------------------------- |
| **Numeric** (default) | Compare as numbers using numeric operators     |
| **String**            | Compare as text strings using string operators |

Only used when bDoValueMatch is true.

Default: `Numeric`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Value Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute on the target to compare against the tag value.

Examples:

* `"Value"`: Compare against "Value" attribute
* `"Threshold"`: Compare against "Threshold" attribute
* `"@Data.Category"`: Compare against data domain attribute

Only used when bDoValueMatch is true.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Numeric Comparison</strong> <code>EPCGExComparison</code></summary>

Comparison operator for numeric values.

| Operator                   | Description                                    |
| -------------------------- | ---------------------------------------------- |
| **Strictly Equal**         | Tag value == Attribute value                   |
| **Nearly Equal** (default) | Tag value ≈ Attribute value (within Tolerance) |
| **Greater Than**           | Tag value > Attribute value                    |
| **Less Than**              | Tag value < Attribute value                    |
| (etc.)                     | See standard numeric comparisons               |

Only used when bDoValueMatch is true and ValueType is Numeric.

Default: `Nearly Equal`

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for numeric comparisons.

Only used when Numeric Comparison is Nearly Equal or Nearly Not Equal.

Default: `DBL_COMPARE_TOLERANCE`

</details>

<details>

<summary><strong>String Comparison</strong> <code>EPCGExStringComparison</code></summary>

Comparison operator for string values.

| Operator               | Description                               |
| ---------------------- | ----------------------------------------- |
| **Strictly Equal**     | Exact string match                        |
| **Contains** (default) | Attribute contains tag value as substring |
| **Starts With**        | Attribute starts with tag value           |
| **Ends With**          | Attribute ends with tag value             |
| (etc.)                 | See standard string comparisons           |

Only used when bDoValueMatch is true and ValueType is String.

Default: `Contains`

</details>

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

**Tip**: This rule is particularly useful when you have metadata stored as tags on one dataset (like categories or classifications) and need to match them against quantitative or categorical attributes on another dataset (like thresholds or filters).

### Outputs

| Pin            | Type       | Description                                      |
| -------------- | ---------- | ------------------------------------------------ |
| **Match Rule** | Match Rule | Match rule factory for tag-to-attribute matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchTagToAttr.h)
