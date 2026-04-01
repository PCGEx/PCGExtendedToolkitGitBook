---
description: Creates a filter definition that compares dot value of a vector and tensors.
icon: circle-dashed
---

# Filter : Tensor Dot

### Overview

The Tensor Dot filter compares the alignment between a vector attribute and the sampled tensor field direction at each point. It computes the dot product between the two vectors and tests the result against configurable thresholds. This is useful for filtering points based on how well they align with a tensor field — for example, selecting points facing along or against a flow direction.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Vector Read**: Reads the operand vector from a point attribute.
2. **Optional Transform**: Optionally transforms the operand by the point's local transform.
3. **Tensor Sampling**: Samples the tensor field at the point's position.
4. **Dot Product**: Computes the dot product between the operand and tensor direction.
5. **Comparison**: Tests the dot product value against the configured threshold and comparison type.

**Usage Notes**

* **Dot Product Range**: Results range from -1 (opposite directions) through 0 (perpendicular) to 1 (same direction).
* **Transform Option**: Use "Transform Operand A" when your attribute is in local space and needs to be converted to world space for comparison.
* **Absolute Dot**: The comparison settings can use absolute value to ignore direction, testing only alignment regardless of which way the vectors point.

### Behavior

```
Vector Attribute: $Transform.Forward
Tensor Field: → → → (pointing right)

Point A: Forward = → (right)    Dot = 1.0   ✓ Pass (aligned)
Point B: Forward = ← (left)     Dot = -1.0  ✗ Fail (opposite)
Point C: Forward = ↑ (up)       Dot = 0.0   ✗ Fail (perpendicular)
```

### Inputs

| Pin         | Type             | Description                                 |
| ----------- | ---------------- | ------------------------------------------- |
| **Tensors** | Tensor Factories | Tensor field definitions to compare against |

### Settings

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Vector operand to compare against the tensor field. This is the direction that will be tested for alignment with the tensor.

</details>

<details>

<summary><strong>Transform Operand A</strong> <code>bool</code></summary>

When enabled, transforms the operand vector by the point's local transform before comparison. Use this when the attribute is in local space.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Dot Comparison Details</strong> <code>FPCGExDotComparisonDetails</code></summary>

Settings controlling how the dot product is evaluated.

| Property             | Type               | Description                                           |
| -------------------- | ------------------ | ----------------------------------------------------- |
| **Comparison**       | `EPCGExComparison` | How to compare (Greater, Less, Equal, etc.)           |
| **Tolerance**        | `double`           | Tolerance for near-equal comparisons                  |
| **Use Absolute Dot** | `bool`             | Use absolute value of dot product, ignoring direction |
| **Threshold**        | `double`           | Value to compare against (-1 to 1)                    |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tensor Sampling Settings</strong> <code>FPCGExTensorHandlerDetails</code></summary>

Settings for how tensors are sampled at each point.

| Property             | Type     | Description                      |
| -------------------- | -------- | -------------------------------- |
| **Invert**           | `bool`   | Invert the tensor direction      |
| **Normalize**        | `bool`   | Normalize the tensor result      |
| **Size**             | `double` | Scale factor for tensor sampling |
| **Sampler Settings** | Struct   | Tensor sampler configuration     |

⚡ PCG Overridable

</details>

#### Inherited Settings

This filter inherits from the point filter factory base.

→ See Filter Factory for common filter options like inversion.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Filter** | Filter Factory | Filter definition for use with filter-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Filters/Points/PCGExTensorDotFilter.h)
