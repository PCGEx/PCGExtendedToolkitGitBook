---
description: Remap a single property or attribute.
icon: circle
---

# Attribute Remap

### Overview

This node remaps attribute values from one range to another with optional clamping before and after the transformation. It supports per-component remapping for vector types, allowing different remap rules for X, Y, Z, and W components. The remapping can use linear interpolation or custom curves for non-linear transformations.

### How It Works

1. **Read Source**: Reads values from the specified source attribute.
2. **Clamp Input (Optional)**: Clamps input values to a specified range before remapping.
3. **Remap**: Transforms values from input range to output range using linear or curve-based interpolation.
4. **Clamp Output (Optional)**: Clamps output values to a specified range after remapping.
5. **Write Target**: Writes remapped values to the target attribute (or overwrites source if same name).

**Usage Notes**

* **Per-Component Rules**: For vector types, you can specify different remap rules for each component (X, Y, Z, W).
* **Scope-Based Ranges**: Input ranges can be computed from the actual data (Min/Max of all values) or specified manually.
* **Integer Casting**: Enable auto-cast to handle integer attributes as doubles for more precise remapping.
* **Curve Support**: Use curves in RemapDetails for non-linear transformations like easing functions.

### Behavior

**Remap Operation:**

```
Source Attribute "Height" with values: [10, 50, 100]

Input Range: 0-100 (from data or manual)
Output Range: 0.0-1.0

Result: [0.1, 0.5, 1.0]

With Input Clamp (min=20):
   10 → clamped to 20 → remapped to 0.2
   50 → remapped to 0.5
   100 → remapped to 1.0
```

### Inputs

| Pin    | Type   | Description                                      |
| ------ | ------ | ------------------------------------------------ |
| **In** | Points | Input point collections with attributes to remap |

### Settings

#### Source/Target Settings

<details>

<summary><strong>Source</strong> <code>FName</code></summary>

The name of the source attribute to read values from.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output To Different Name</strong> <code>bool</code></summary>

When enabled, writes remapped values to a different attribute instead of overwriting the source.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target</strong> <code>FName</code></summary>

The name of the target attribute to write remapped values to.

📋 _Visible when Output To Different Name is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Auto Cast Integer To Double</strong> <code>bool</code></summary>

When enabled, automatically converts integer attributes to double for remapping, providing more precise results.

Default: `false`

</details>

#### Remap Rule (Default)

The default remap rule applies to single-component values, or to all components of vectors unless individual overrides are specified.

<details>

<summary><strong>Clamp Input</strong> <code>FPCGExClampDetails</code></summary>

Optional clamping applied to input values before remapping.

| Property    | Type   | Description                   |
| ----------- | ------ | ----------------------------- |
| **Enabled** | bool   | Whether to clamp input values |
| **Min**     | double | Minimum clamp value           |
| **Max**     | double | Maximum clamp value           |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remap Details</strong> <code>FPCGExRemapDetails</code></summary>

Controls how values are remapped from input range to output range.

| Property      | Type        | Description                                        |
| ------------- | ----------- | -------------------------------------------------- |
| **Range Min** | enum/double | Source of minimum input value (FromData, Constant) |
| **Range Max** | enum/double | Source of maximum input value (FromData, Constant) |
| **Remap Min** | double      | Minimum output value                               |
| **Remap Max** | double      | Maximum output value                               |
| **Curve**     | UCurveFloat | Optional curve for non-linear remapping            |
| **Truncate**  | bool        | Whether to truncate to integer after remapping     |
| **Snap**      | double      | Optional snap increment for output values          |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Clamp Output</strong> <code>FPCGExClampDetails</code></summary>

Optional clamping applied to output values after remapping.

| Property    | Type   | Description                    |
| ----------- | ------ | ------------------------------ |
| **Enabled** | bool   | Whether to clamp output values |
| **Min**     | double | Minimum clamp value            |
| **Max**     | double | Maximum clamp value            |

⚡ PCG Overridable

</details>

#### Individual Component Overrides

For vector attributes (FVector2D, FVector, FVector4), you can specify different remap rules per component.

<details>

<summary><strong>Override Component 2</strong> <code>bool</code></summary>

When enabled, uses a separate remap rule for the second (Y) component.

</details>

<details>

<summary><strong>Remap (2nd Component)</strong> <code>FPCGExComponentRemapRule</code></summary>

Remap rule for the Y component of vector values.

📋 _Visible when Override Component 2 is enabled_

</details>

<details>

<summary><strong>Override Component 3</strong> <code>bool</code></summary>

When enabled, uses a separate remap rule for the third (Z) component.

</details>

<details>

<summary><strong>Remap (3rd Component)</strong> <code>FPCGExComponentRemapRule</code></summary>

Remap rule for the Z component of vector values.

📋 _Visible when Override Component 3 is enabled_

</details>

<details>

<summary><strong>Override Component 4</strong> <code>bool</code></summary>

When enabled, uses a separate remap rule for the fourth (W) component.

</details>

<details>

<summary><strong>Remap (4th Component)</strong> <code>FPCGExComponentRemapRule</code></summary>

Remap rule for the W component of FVector4 values.

📋 _Visible when Override Component 4 is enabled_

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| **Out** | Points | Point collections with remapped attribute values |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExAttributeRemap.h)
