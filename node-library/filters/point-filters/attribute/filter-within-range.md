---
description: Checks if a value is within a given range.
icon: circle-dashed
---

# Filter : Within Range

### Overview

This filter tests whether a numeric attribute value on each point falls within a specified range. The range can be defined as constant min/max values, or read from an external attribute set as FVector2 attributes (where X is min and Y is max), allowing multiple ranges to be tested at once. Points whose value falls within any of the defined ranges pass the filter.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Value Reading**: Reads the numeric value of Operand A from each point (converted to double).
2. **Range Setup**: Builds one or more ranges from either the constant min/max or from FVector2 attributes on external data.
3. **Range Test**: Tests whether the value falls within any of the defined ranges.
4. **Result**: Returns pass if the value is within range (or outside, if inverted).

**Usage Notes**

* **Multiple Ranges**: When using the Attribute Set source, each FVector2 attribute defines a separate range (X=min, Y=max). The value passes if it falls within **any** of these ranges.
* **Inclusive vs Exclusive**: By default the range boundaries are exclusive (strictly between min and max). Enable Inclusive to include the boundary values themselves.

### Behavior

**Constant Range:**

```
Operand A: $Height
Range: [-100, 100]
Inclusive: false

Point 0: Height= -50 → -100 < -50 < 100: Pass
Point 1: Height= 100 → 100 is not < 100: Fail (exclusive)
Point 2: Height= 150 → 150 > 100: Fail

With Inclusive = true:
Point 1: Height= 100 → -100 <= 100 <= 100: Pass
```

**Attribute Set (Multiple Ranges):**

```
Attribute Set FVector2 ranges:
  RangeA: (0, 10)
  RangeB: (50, 60)

Point: Value=5  → within RangeA: Pass
Point: Value=25 → in neither: Fail
Point: Value=55 → within RangeB: Pass
```

### Inputs

| Pin        | Type          | Description                                                      |
| ---------- | ------------- | ---------------------------------------------------------------- |
| **Ranges** | Attribute Set | External data providing FVector2 range attributes (X=min, Y=max) |

📋 _Visible when Source = Attribute Set_

### Settings

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read and test against the range. Value is converted to double.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Source</strong> <code>EPCGExRangeSource</code></summary>

Where to read the range definition from.

| Option            | Description                                                            |
| ----------------- | ---------------------------------------------------------------------- |
| **Constant**      | Use constant min/max values                                            |
| **Attribute Set** | Read FVector2 attributes from an external attribute set (X=min, Y=max) |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attributes</strong> <code>TArray&#x3C;FPCGAttributePropertyInputSelector></code></summary>

List of FVector2 attributes to read ranges from. Each attribute defines a separate range where X is min and Y is max.

⚡ PCG Overridable

📋 _Visible when Source = Attribute Set_

</details>

<details>

<summary><strong>Range Min</strong> <code>double</code></summary>

The minimum value of the range.

Default: `-100`

⚡ PCG Overridable

📋 _Visible when Source = Constant_

</details>

<details>

<summary><strong>Range Max</strong> <code>double</code></summary>

The maximum value of the range.

Default: `100`

⚡ PCG Overridable

📋 _Visible when Source = Constant_

</details>

<details>

<summary><strong>Inclusive</strong> <code>bool</code></summary>

Whether the range boundaries are inclusive. When disabled, values must be strictly between min and max. When enabled, values equal to min or max also pass.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the result, so the filter passes when the value is **outside** the range.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExWithinRangeFilter.h)
