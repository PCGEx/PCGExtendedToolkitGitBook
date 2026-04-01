---
description: Reduce @Data domain attribute.
icon: circle
---

# Reduce Data

### Overview

This node aggregates @Data domain attribute values from multiple input collections into a single output value. It supports various reduction methods including min, max, sum, average, join (for strings), and hash operations. The result is written to the @Data domain of the output.

### How It Works

1. **Gather Inputs**: Collects the specified @Data attribute from all input collections.
2. **Reduce Values**: Applies the selected reduction method to combine all values.
3. **Output Result**: Writes the reduced value to the @Data domain of the output.

**Usage Notes**

* **@Data Domain**: This node operates on collection-level attributes, not point attributes.
* **Type Conversion**: Use Custom Output Type to convert the result to a different type (not supported for Join).
* **Hash Methods**: Hash produces an order-dependent result; Hash (Sorted) produces consistent results regardless of input order.
* **Join**: Concatenates string representations of all values with the specified delimiter.

### Behavior

```
Reduce Operation:

Input Collections with @Data.Score attribute:
   Collection A: Score = 100
   Collection B: Score = 50
   Collection C: Score = 75

Method = Min:    Result = 50
Method = Max:    Result = 100
Method = Sum:    Result = 225
Method = Average: Result = 75
Method = Join:   Result = "100, 50, 75"
```

### Inputs

| Pin    | Type | Description                                       |
| ------ | ---- | ------------------------------------------------- |
| **In** | Any  | Input collections with @Data attributes to reduce |

### Settings

#### Attribute Settings

<details>

<summary><strong>Source</strong> <code>FName</code></summary>

The @Data domain attribute name to read values from.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output To Different Name</strong> <code>bool</code></summary>

When enabled, writes the reduced value to a different attribute name.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target</strong> <code>FName</code></summary>

The attribute name to write the reduced value to.

📋 _Visible when Output To Different Name is enabled_

⚡ PCG Overridable

</details>

#### Reduction Settings

<details>

<summary><strong>Method</strong> <code>EPCGExReduceDataDomainMethod</code></summary>

The reduction method to apply to the collected values.

| Option            | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| **Min**           | Returns the minimum value                                     |
| **Max**           | Returns the maximum value                                     |
| **Sum**           | Returns the sum of all values                                 |
| **Average**       | Returns the arithmetic mean                                   |
| **Join**          | Concatenates string representations with delimiter            |
| **Hash**          | Computes hash in input order                                  |
| **Hash (Sorted)** | Sorts values first, then computes hash for consistent results |

Default: `Min`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom Output Type</strong> <code>bool</code></summary>

When enabled, converts the result to a specified type. Not supported for Join method.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Type</strong> <code>EPCGMetadataTypes</code></summary>

The target type for the reduced value.

Default: `Integer32`

📋 _Visible when Custom Output Type is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Join Delimiter</strong> <code>FString</code></summary>

The string used to separate values when using the Join method.

Default: `,`

📋 _Visible when Method = Join_

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type | Description                         |
| ------- | ---- | ----------------------------------- |
| **Out** | Any  | Output with reduced @Data attribute |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExReduceDataAttribute.h)
