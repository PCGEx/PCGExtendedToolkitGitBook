---
description: >-
  Filters edges by comparing a string attribute value between the edge's start
  and end vertices.
icon: circle-dashed
---

# Edge Filter : Endpoints Compare (String)

### Overview

This edge filter reads a string attribute from both endpoints of each edge and compares them using the specified comparison mode. This allows you to filter edges based on string relationships between endpoint values — for example, keeping only edges where both endpoints have the same tag, or edges connecting vertices with different categories.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads the specified string attribute from both the start and end vertices of each edge.
2. **Comparison**: Compares the two strings using the selected comparison mode.
3. **Result**: The edge passes or fails based on the comparison result and invert setting.

**Usage Notes**

* **Attribute Source**: The same attribute is read from both endpoints.
* **Case Sensitivity**: Some comparison modes are case-sensitive, others are not.
* **Contains Modes**: Allow checking if one string contains the other.

### Behavior

```
Comparison Examples (Attribute = "Type"):

StrictlyEqual:
  ["Rock"]---["Rock"]  → PASS
  ["Rock"]---["Tree"]  → FAIL

Contains:
  ["BigRock"]---["Rock"] → PASS (BigRock contains Rock)
  ["Tree"]---["Rock"]    → FAIL
```

### Settings

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The string attribute to read from both edge endpoints for comparison.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExStringComparison</code></summary>

The comparison mode to use when comparing start vertex value against end vertex value.

| Option                     | Description                                 |
| -------------------------- | ------------------------------------------- |
| **StrictlyEqual**          | Strings must match exactly (case-sensitive) |
| **StrictlyNotEqual**       | Strings must not match                      |
| **LengthStrictlyEqual**    | String lengths must be equal                |
| **LengthStrictlyNotEqual** | String lengths must differ                  |
| **Contains**               | Start string contains end string            |
| **StartsWith**             | Start string begins with end string         |
| **EndsWith**               | Start string ends with end string           |

Default: `StrictlyEqual`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the filter result (pass becomes fail and vice versa).

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Edges/PCGExEdgeEndpointsCompareStrFilter.h)
