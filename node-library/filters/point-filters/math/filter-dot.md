---
description: Compares the dot value of two vectors.
icon: circle-dashed
---

# Filter : Dot

### Overview

This filter evaluates points by computing the dot product between two vector operands and comparing it against a threshold. The dot product measures directional alignment between vectors, where 1.0 indicates parallel vectors, 0.0 indicates perpendicular vectors, and -1.0 indicates opposite directions. Both operands can optionally be transformed by the point's local transform before comparison.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Vector Reading**: Reads the first vector from Operand A attribute for each point.
2. **Comparison Vector**: Gets the second vector from either a constant or Operand B attribute.
3. **Transform Application**: Optionally transforms each operand by the point's local transform.
4. **Dot Calculation**: Computes the dot product between the two (normalized) vectors.
5. **Threshold Comparison**: Compares the result against the threshold using the selected comparison operator.
6. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Vector Normalization**: Vectors are normalized before the dot product is calculated.
* **Unsigned Mode**: When enabled, uses the absolute value of the dot product, treating opposite directions as equivalent.
* **Domain Options**: Compare using raw scalar (-1 to 1) or degrees (0 to 180).

### Behavior

**Dot Product Values:**

```
Parallel vectors (same dir):      dot = 1.0   (0 degrees)
Perpendicular vectors:            dot = 0.0   (90 degrees)
Opposite vectors (opposite dir):  dot = -1.0  (180 degrees)

With Unsigned Comparison:
Parallel OR opposite:             |dot| = 1.0 (treated same)
Perpendicular:                    |dot| = 0.0
```

**Example Comparison:**

```
Operand A: Point's Up vector
Operand B: World Up (0, 0, 1)

Point A (upright):    dot = 1.0  → GreaterOrEqual 0.5: Pass
Point B (tilted 45°): dot = 0.7  → GreaterOrEqual 0.5: Pass
Point C (sideways):   dot = 0.0  → GreaterOrEqual 0.5: Fail
Point D (inverted):   dot = -1.0 → GreaterOrEqual 0.5: Fail
```

### Settings

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The vector attribute to read as Operand A.

</details>

<details>

<summary><strong>Transform Operand A</strong> <code>bool</code></summary>

Transform Operand A with the local point's transform before computing the dot product.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert (Operand A)</strong> <code>bool</code></summary>

Negate Operand A before computing the dot product.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant vector or read from an attribute for Operand B.

| Option        | Description                       |
| ------------- | --------------------------------- |
| **Constant**  | Use the specified constant vector |
| **Attribute** | Read from a point attribute       |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The vector attribute to read as Operand B.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Invert (Operand B)</strong> <code>bool</code></summary>

Negate Operand B before computing the dot product.

Default: `false`

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>FVector</code></summary>

The constant vector to use as Operand B.

Default: `(0, 0, 1)` (World Up)

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Transform Operand B</strong> <code>bool</code></summary>

Transform Operand B with the local point's transform before computing the dot product.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Dot Comparison Details</strong> <code>FPCGExDotComparisonDetails</code></summary>

Configuration for the dot product comparison.

**Domain** `EPCGExAngularDomain`

* **Scalar**: Compare using raw dot product value (-1 to 1)
* **Degrees**: Compare using angle in degrees (0 to 180)

**Comparison** `EPCGExComparison` The comparison operator to use.

**Unsigned Comparison** `bool` When enabled, uses the absolute value of the dot product, treating parallel and anti-parallel vectors as equivalent.

**Threshold Input** `EPCGExInputValueType` Whether to use a constant threshold or read from an attribute.

**Scalar** `double` (Default: `0.5`) The dot product threshold when using Scalar domain.

**Degrees** `double` (Default: `90`) The angle threshold when using Degrees domain.

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExDotFilter.h)
