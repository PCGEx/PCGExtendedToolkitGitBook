---
description: Outputs predefined mathematical and vector constants as PCG data.
icon: circle
---

# Constant

### Overview

This node generates param data containing predefined constant values from selected categories such as mathematical constants (π, e), common numbers (0, 1, 2), angles (90°, 180°), and vector directions (Up, Forward, Right). The output can be transformed through negation, reciprocal conversion, or custom multiplication, and supports multiple numeric output formats for compatibility with different attribute types.

### How It Works

1. **List Selection**: Chooses a predefined constant list (e.g., Irrationals, Angles, Vectors)
2. **Value Generation**: Creates param data with the constants from the selected list
3. **Transformations**: Optionally applies negation, reciprocal, or custom multiplication to values
4. **Type Conversion**: Converts numeric values to the specified output type (Double, Float, Int32, Int64)
5. **Attribute Naming**: Applies custom attribute name mapping if provided
6. **Output Staging**: Produces param data with the constant values

### Behavior

**Constant Lists:**

```
ZeroAndOne → Outputs: 0, 1
MinusOne → Outputs: -1
Twos → Outputs: 0.5, 2
Tens → Outputs: 10, 100, 1000, etc.
Irrationals → Outputs: π (3.14159...), e (2.71828...), √2, etc.
Angles → Outputs: 45°, 90°, 180°, 360° (in degrees or radians)
Vectors → Outputs: Up(0,0,1), Forward(1,0,0), Right(0,1,0), etc.
Booleans → Outputs: True, False
```

**Transformations:**

```
Original: 2.0

NegateOutput = true:
  Result: -2.0

OutputReciprocal = true:
  Result: 0.5 (1/2)

CustomMultiplier = 3.5:
  Result: 7.0 (2 * 3.5)

Combined (Negate + Reciprocal + Multiplier):
  Step 1: Negate → -2.0
  Step 2: Reciprocal → -0.5
  Step 3: Multiply by 3.5 → -1.75
```

**Numeric Output Types:**

```
Original: 3.14159 (π)

Double: 3.14159 (full precision)
Float: 3.14159 (32-bit precision)
Int32: 3 (truncated to integer)
Int64: 3 (truncated to 64-bit integer)
```

```
Attribute Name Mapping:

Default names: "Pi", "E", "Sqrt2"
Custom map: {"Pi" → "MyPi", "E" → "Euler"}
Output: "MyPi", "Euler", "Sqrt2"
```

Good for: providing standard math constants, direction vectors, common multipliers, reference values, unit conversions

### Settings

<details>

<summary><strong>Constant List</strong> <code>EPCGExConstantListID</code></summary>

Which category of constants to output.

| Option             | Description                                  |
| ------------------ | -------------------------------------------- |
| **0 and 1**        | Zero and one                                 |
| **-1**             | Minus one                                    |
| **0.5 and 2**      | Half and two                                 |
| **Powers of 10**   | 10, 100, 1000, etc.                          |
| **Irrationals**    | π, e, √2, golden ratio, etc.                 |
| **Angles**         | Common angles (45°, 90°, 180°, 360°)         |
| **0**              | Zero only                                    |
| **1**              | One only                                     |
| **Axes**           | Direction vectors (Up, Forward, Right, etc.) |
| **True and False** | Boolean values                               |
| **True**           | True only                                    |
| **False**          | False only                                   |
| **Unit Vector**    | (1,1,1) vector                               |
| **Zero Vector**    | (0,0,0) vector                               |
| **Half Vector**    | (0.5,0.5,0.5) vector                         |
| **Up Vector**      | (0,0,1) vector                               |
| **Right Vector**   | (0,1,0) vector                               |
| **Forward Vector** | (1,0,0) vector                               |
| **2**              | Two only                                     |
| **0.5**            | Half only                                    |

</details>

<details>

<summary><strong>Negate Output</strong> <code>bool</code></summary>

Whether to negate (flip sign) all output values.

* **false** (default): Output values as-is
* **true**: Multiply all values by -1

Applied before reciprocal conversion and custom multiplier.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Reciprocal</strong> <code>bool</code></summary>

Whether to output the reciprocal (1/value) of each constant.

* **false** (default): Output original values
* **true**: Output 1/value for each constant

Applied after negation, before custom multiplier.

Examples:

* 2 becomes 0.5
* 0.5 becomes 2
* π becomes 1/π (≈0.318)

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom Multiplier</strong> <code>double</code></summary>

A custom multiplier to apply to all output values.

Applied after negation and reciprocal conversion.

Examples:

* `1.0` (default): No change
* `100.0`: Convert to percentage scale
* `0.01745`: Convert degrees to radians
* `57.2958`: Convert radians to degrees

Default: `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Numeric Output Type</strong> <code>EPCGExNumericOutput</code></summary>

The numeric type to use for numeric constant outputs (not applicable to vectors or booleans).

| Type       | Description                            |
| ---------- | -------------------------------------- |
| **Double** | 64-bit floating point (full precision) |
| **Float**  | 32-bit floating point                  |
| **Int32**  | 32-bit integer (truncates decimals)    |
| **Int64**  | 64-bit integer (truncates decimals)    |

Integer types will truncate decimal values (3.14 becomes 3).

</details>

<details>

<summary><strong>Attribute Name Map</strong> <code>TMap</code></summary>

Optional mapping to rename constant attributes in the output.

Maps from default constant names to custom names.

Examples:

* `{"Pi" → "CircleRatio"}`: Renames Pi attribute
* `{"E" → "Euler", "Pi" → "Tau"}`: Renames multiple constants

Unmapped constants retain their default names.

Default: `{}` (empty map)

</details>

### Outputs

| Pin     | Type       | Description                              |
| ------- | ---------- | ---------------------------------------- |
| **Out** | Param Data | Constant values as param data attributes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Constants/PCGExConstants.h)
