---
description: Compares the distance from each point to the nearest target.
icon: circle-dashed
---

# Filter : Distance

### Overview

This filter evaluates points based on their distance to a set of target points provided via an input pin. It finds the nearest target for each input point and compares that distance against a threshold using configurable comparison operators. Useful for proximity-based filtering, exclusion zones, or distance-dependent effects.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Target Collection**: Gathers target points from the connected "Targets" input.
2. **Distance Calculation**: For each input point, calculates the distance to the nearest target.
3. **Comparison**: Compares the calculated distance against the threshold value.
4. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Targets Input**: Requires a "Targets" input pin with point data defining the reference locations.
* **Self-Filtering**: Can optionally ignore self-collection when targets overlap with input.
* **Distance Methods**: Supports various distance calculation methods including center-to-center and bounds-based.

### Behavior

**Distance Comparison Examples:**

```
Targets: [Point at (0,0,0)]
Threshold: 100

Point A (50, 0, 0):  Distance = 50  → LessThan 100: Pass
Point B (100, 0, 0): Distance = 100 → LessThan 100: Fail
Point C (200, 0, 0): Distance = 200 → LessThan 100: Fail

With NearlyEqual (Tolerance=10):
Point B: |100 - 100| ≤ 10 → Pass
```

### Inputs

| Pin         | Type   | Description                               |
| ----------- | ------ | ----------------------------------------- |
| **Targets** | Points | Target points to measure distance against |

### Settings

<details>

<summary><strong>Distance Details</strong> <code>FPCGExDistanceDetails</code></summary>

Configuration for how distances are measured between source and target points. Includes options for center-to-center, bounds-based, and other distance calculation methods.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when testing distance against the threshold.

| Option                    | Description                                     |
| ------------------------- | ----------------------------------------------- |
| **Equal**                 | Distance must exactly equal threshold           |
| **Not Equal**             | Distance must not equal threshold               |
| **Greater Than**          | Distance must exceed threshold                  |
| **Greater Than Or Equal** | Distance must be at least threshold             |
| **Less Than**             | Distance must be below threshold                |
| **Less Than Or Equal**    | Distance must be at most threshold              |
| **Nearly Equal**          | Distance must be within tolerance of threshold  |
| **Nearly Not Equal**      | Distance must be outside tolerance of threshold |

Default: `Nearly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant threshold or read from an attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use the specified constant value      |
| **Attribute** | Read threshold from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Distance Threshold (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the distance threshold from.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Distance Threshold</strong> <code>double</code></summary>

The constant distance threshold to compare against.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for the comparison.

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

📋 _Visible when Comparison is Nearly Equal or Nearly Not Equal_

</details>

<details>

<summary><strong>Ignore Self</strong> <code>bool</code></summary>

When enabled, a collection will never be tested against itself.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Check Against Data Bounds</strong> <code>bool</code></summary>

When enabled with collection filter, uses collection bounds as a proxy point instead of per-point testing.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy, Missing Data Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExDistanceFilter.h)
