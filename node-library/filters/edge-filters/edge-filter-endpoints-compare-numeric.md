---
description: >-
  Filters edges by comparing a numeric attribute value between the edge's start
  and end vertices.
icon: circle-dashed
---

# Edge Filter : Endpoints Compare (Numeric)

### Overview

This edge filter reads a numeric attribute from both endpoints of each edge and compares them using the specified comparison operator. This allows you to filter edges based on relationships between endpoint values — for example, keeping only edges where the start vertex has a higher value than the end vertex, or edges connecting vertices with similar values.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads the specified numeric attribute from both the start and end vertices of each edge.
2. **Comparison**: Compares the two values using the selected comparison operator.
3. **Result**: The edge passes or fails based on the comparison result and invert setting.

**Usage Notes**

* **Attribute Source**: The same attribute is read from both endpoints.
* **Comparison Direction**: Start value is compared against End value (Start > End, Start < End, etc.).
* **Tolerance**: For "Nearly Equal" comparisons, the tolerance controls how close values must be.

### Behavior

```
Comparison Examples (Attribute = "Height"):

StrictlyGreater (Start > End):
  [H=10]---[H=5]  → PASS (10 > 5)
  [H=5]---[H=10]  → FAIL (5 > 10)

NearlyEqual (Tolerance = 1):
  [H=10]---[H=10.5] → PASS (within tolerance)
  [H=10]---[H=15]   → FAIL (difference > tolerance)
```

### Settings

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The numeric attribute to read from both edge endpoints for comparison.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when comparing start vertex value against end vertex value.

| Option               | Description                      |
| -------------------- | -------------------------------- |
| **StrictlyGreater**  | Start > End                      |
| **StrictlySmaller**  | Start < End                      |
| **EqualOrGreater**   | Start >= End                     |
| **EqualOrSmaller**   | Start <= End                     |
| **StrictlyEqual**    | Start == End                     |
| **StrictlyNotEqual** | Start != End                     |
| **NearlyEqual**      | Start ≈ End (within tolerance)   |
| **NearlyNotEqual**   | Start !≈ End (outside tolerance) |

Default: `StrictlyGreater`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance value for approximate comparison modes (NearlyEqual, NearlyNotEqual).

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison = NearlyEqual or NearlyNotEqual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the filter result (pass becomes fail and vice versa).

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Edges/PCGExEdgeEndpointsCompareNumFilter.h)
