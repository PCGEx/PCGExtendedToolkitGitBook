---
description: Test an aspect of the collection's bounds against a comparison value.
icon: circle-dashed
---

# Data Filter : Bounds

### Overview

This filter evaluates geometric properties of a data collection's bounding box. It can test various bound aspects such as volume, extents, size ratios, and individual axis values against constant or attribute-based comparison values. The filter operates at the collection level, passing or failing entire point sets.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Aspect Extraction**: Extracts the selected geometric property from the collection's bounds.
2. **Component Selection**: For vector-based aspects, extracts the specified component (length, X, Y, or Z).
3. **Comparison**: Compares the extracted value against the operand using the selected comparison operator.
4. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Collection Filter**: This filter evaluates at the data set level, not per-point.
* **Bounds Calculation**: Uses the combined bounding box of all points in the collection.
* **Aspect Ratios**: Useful for filtering based on collection shape (flat, elongated, cubic).

### Behavior

**Volume Check Example:**

```
Collection bounds: 100x50x25

Config: Aspect=Volume, OperandB > 100000
Volume = 100 * 50 * 25 = 125000
Result: Pass (125000 > 100000)
```

**Ratio Check Example:**

```
Collection bounds: 100x50x25

Config: Aspect=Ratio (XY), OperandB > 1.5
Ratio XY = 100/50 = 2.0
Result: Pass (2.0 > 1.5)
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>EPCGExDataBoundsAspect</code></summary>

The bounds aspect to test.

| Option             | Description                                    |
| ------------------ | ---------------------------------------------- |
| **Extents**        | Half-size of the bounding box (center to edge) |
| **Min**            | Minimum corner of the bounding box             |
| **Max**            | Maximum corner of the bounding box             |
| **Size**           | Full size of the bounding box                  |
| **Volume**         | Total volume of the bounding box               |
| **Ratio**          | Aspect ratio between two axes                  |
| **Ratio (Sorted)** | Aspect ratio of largest to smallest axis       |

Default: `Volume`

</details>

<details>

<summary><strong>Sub Operand</strong> <code>EPCGExDataBoundsComponent</code></summary>

Which component to extract from vector-based aspects.

| Option             | Description                             |
| ------------------ | --------------------------------------- |
| **Length**         | Vector length (magnitude)               |
| **Length Squared** | Squared vector length (faster, no sqrt) |
| **X**              | X component                             |
| **Y**              | Y component                             |
| **Z**              | Z component                             |

Default: `Length`

📋 _Visible when Operand A is Extents, Min, Max, or Size_

</details>

<details>

<summary><strong>Ratio</strong> <code>EPCGExDataBoundsRatio</code></summary>

Which axis pair to use for aspect ratio calculation.

| Option | Description    |
| ------ | -------------- |
| **XY** | X divided by Y |
| **XZ** | X divided by Z |
| **YZ** | Y divided by Z |
| **YX** | Y divided by X |
| **ZX** | Z divided by X |
| **ZY** | Z divided by Y |

Default: `XY`

📋 _Visible when Operand A = Ratio_

</details>

<details>

<summary><strong>Operand B</strong> <code>FPCGExCompareSelectorDouble</code></summary>

The comparison configuration including operator and value to compare against. Can use a constant value or read from an attribute.

⚡ PCG Overridable

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Collections/PCGExDataBoundsFilter.h)
