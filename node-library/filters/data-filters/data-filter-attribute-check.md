---
description: Simple attribute existence check for data collections.
icon: circle-dashed
---

# Data Filter : Attribute Check

### Overview

This filter tests whether a specified attribute exists on the input data. It operates at the collection level, evaluating entire point sets rather than individual points. The filter can check for exact attribute names or use pattern matching, and optionally verify the attribute's data type.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Name Matching**: Compares the target attribute name against attributes in the data using the selected match mode.
2. **Domain Check**: Optionally verifies the attribute exists in a specific domain (data or elements).
3. **Type Verification**: When enabled, confirms the attribute matches the expected data type.
4. **Result**: Returns pass if the attribute exists (with all conditions met), fail otherwise.

**Usage Notes**

* **Collection Filter**: This filter evaluates at the data set level, not per-point. It passes or fails entire collections.
* **Pattern Matching**: Use wildcards or partial matching to check for attribute name patterns.
* **Type Safety**: Enable type checking when downstream nodes require specific attribute types.

### Behavior

**Attribute Check Logic:**

```
Data has attribute "Health" (type: Double)

Config: Name="Health", Match=Equals, CheckType=false
Result: Pass

Config: Name="Health", Match=Equals, CheckType=true, Type=Integer
Result: Fail (type mismatch)

Config: Name="HP", Match=Equals
Result: Fail (name mismatch)

Config: Name="*", Match=Contains
Result: Pass (any attribute exists)
```

### Settings

<details>

<summary><strong>Attribute Name</strong> <code>FString</code></summary>

The attribute name to search for. Interpretation depends on the Match mode setting.

Default: `Name`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Domain</strong> <code>EPCGExAttribtueDomainCheck</code></summary>

Which attribute domain to check.

| Option       | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| **Any**      | Ignore domain, check all attributes                          |
| **Data**     | Only check data-level attributes                             |
| **Elements** | Only check element-level (point) attributes                  |
| **Match**    | Domain must match (requires domain prefix in attribute name) |

Default: `Any`

</details>

<details>

<summary><strong>Match</strong> <code>EPCGExStringMatchMode</code></summary>

How to compare the attribute name.

| Option         | Description                                  |
| -------------- | -------------------------------------------- |
| **Equals**     | Exact match required                         |
| **Contains**   | Attribute name contains the search string    |
| **StartsWith** | Attribute name starts with the search string |
| **EndsWith**   | Attribute name ends with the search string   |

Default: `Equals`

</details>

<details>

<summary><strong>Do Check Type</strong> <code>bool</code></summary>

Whether to also verify the attribute's data type matches the expected type.

Default: `false`

</details>

<details>

<summary><strong>Type</strong> <code>EPCGMetadataTypes</code></summary>

The required attribute type when type checking is enabled.

Default: `Unknown`

📋 _Visible when Do Check Type = true_

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the result of this filter. When enabled, passes become fails and vice versa.

Default: `false`

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                         | Description                   |
| ---------- | ---------------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Collection) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Collections/PCGExAttributeCheckFilter.h)
