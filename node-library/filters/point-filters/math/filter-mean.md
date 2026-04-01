---
description: Compares values against their mean.
icon: circle-dashed
---

# Filter : Mean

### Overview

This filter evaluates points by comparing an attribute value against a statistical mean calculated from all points in the collection. It first computes a reference value (average, median, mode, or central value) across the entire dataset, then tests each individual point's value against thresholds relative to that reference. This enables filtering based on statistical deviation from the norm.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Collection**: Reads the target attribute value from all points.
2. **Mean Calculation**: Computes the reference value using the selected statistical method.
3. **Threshold Application**: Applies below/above thresholds relative to the reference.
4. **Comparison**: Tests each point's value against the computed thresholds.
5. **Result**: Returns pass if the value falls within the acceptable range.

**Usage Notes**

* **Collection-Wide**: The mean is calculated from all points before individual testing.
* **Relative vs Discrete**: Relative mode normalizes values to 0-1 range; Discrete uses raw values.
* **Mode Tolerance**: When using ModeMin/ModeMax, tolerance groups similar values together.

### Behavior

**Mean Methods:**

```
Values: [10, 20, 30, 40, 50]

Average:  (10+20+30+40+50) / 5 = 30
Median:   Middle value = 30
Central:  (Min + Max) / 2 = (10+50) / 2 = 30
ModeMin:  Most common value (with tolerance), use smallest
ModeMax:  Most common value (with tolerance), use largest
Fixed:    User-specified value
```

**Exclusion Example:**

```
Mean: 30
ExcludeBelow: 0.2  (relative)
ExcludeAbove: 0.2  (relative)

With Relative mode:
- Range: [30 - 30*0.2, 30 + 30*0.2] = [24, 36]
- Values 24-36: Pass
- Values <24 or >36: Fail
```

### Settings

<details>

<summary><strong>Target</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read and compare. Values are converted to double internally.

</details>

<details>

<summary><strong>Measure</strong> <code>EPCGExMeanMeasure</code></summary>

How to interpret threshold values.

| Option       | Description                                     |
| ------------ | ----------------------------------------------- |
| **Relative** | Thresholds are relative to the mean (0-1 scale) |
| **Discrete** | Thresholds use absolute values                  |

Default: `Relative`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mean Method</strong> <code>EPCGExMeanMethod</code></summary>

Which statistical method to use for calculating the reference value.

| Option      | Description                         |
| ----------- | ----------------------------------- |
| **Average** | Arithmetic mean of all values       |
| **Median**  | Middle value when sorted            |
| **ModeMin** | Most common value (smallest if tie) |
| **ModeMax** | Most common value (largest if tie)  |
| **Central** | (Min + Max) / 2                     |
| **Fixed**   | Use a user-specified constant value |

Default: `Average`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mean Value</strong> <code>double</code></summary>

The fixed reference value to use when Mean Method is set to Fixed.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Mean Method = Fixed_

</details>

<details>

<summary><strong>Mode Tolerance</strong> <code>double</code></summary>

Tolerance for grouping similar values when estimating the mode. Values within this tolerance are considered equivalent.

Default: `5`

⚡ PCG Overridable

📋 _Visible when Mean Method = ModeMin or ModeMax_

</details>

<details>

<summary><strong>Exclude Below Mean</strong> <code>bool</code></summary>

When enabled, excludes points whose value falls below the threshold relative to the mean.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Exclude Below</strong> <code>double</code></summary>

Threshold for excluding values below the mean. In Relative mode, this is a fraction of the mean. In Discrete mode, this is an absolute offset.

Default: `0.2`

⚡ PCG Overridable

📋 _Visible when Exclude Below Mean is enabled_

</details>

<details>

<summary><strong>Exclude Above Mean</strong> <code>bool</code></summary>

When enabled, excludes points whose value exceeds the threshold relative to the mean.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Exclude Above</strong> <code>double</code></summary>

Threshold for excluding values above the mean. In Relative mode, this is a fraction of the mean. In Discrete mode, this is an absolute offset.

Default: `0.2`

⚡ PCG Overridable

📋 _Visible when Exclude Above Mean is enabled_

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the result of the test.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExMeanFilter.h)
