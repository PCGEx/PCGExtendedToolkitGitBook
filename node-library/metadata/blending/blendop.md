---
description: Creates a single Blend Operation node to be used with Blend Op pins.
icon: circle-dashed
---

# BlendOp

### Overview

This provider node creates a configured blend operation that defines how attributes should be combined between two operands (A and B). The operation can read from point properties or custom attributes, apply weighted blending using various modes (average, lerp, copy, etc.), and write results to new or existing attributes. Multiple blend operations can be combined in the Attribute Blender node to process different attributes with different blending strategies.

### How It Works

1. **Operation Configuration**: Defines operand sources (A and B), blend mode, and output destination.
2. **Weight Setup**: If the blend mode requires weighting (e.g., Lerp), configures weight source as constant, attribute, or curve-remapped value.
3. **Factory Creation**: Creates a UPCGExBlendOpFactory instance configured with these settings.
4. **Output to Blender**: The factory is passed to an Attribute Blender node which executes the blend operation on actual point data.

**Usage Notes**

* **Operand B Reuse**: If "Use Operand B" is disabled, the blend operation uses Operand A's input for both sides. This is useful for modes like "Copy" where you only need one source.
* **Output Modes**: "Same as A" writes results back to Operand A's attribute, while "New" creates a fresh attribute. "Transient" creates a temporary attribute available only during the current operation.
* **Weight Requirement**: Some blend modes (Average, Min, Max, Copy) ignore weight settings. Others (Lerp, Mix, Sum) require valid weight configuration.
* **Inline Constants**: Operands can reference inline constant values from connected Labels pins, allowing blending with fixed values rather than only attributes.

### Behavior

**Blend Operation Creation:**

```
Provider Node              Blender Node
┌────────────┐            ┌────────────┐
│ BlendOp    │  factory   │ Attribute  │   result    Output
│ Config     │ ─────────► │ Blender    │ ──────────► Points
└────────────┘            └────────────┘

Example: Lerp Mode with Weight = 0.5
  Operand A: [1.0, 2.0, 3.0]
  Operand B: [5.0, 6.0, 7.0]
  Weight:    [0.5, 0.5, 0.5]
  Result:    [3.0, 4.0, 5.0]  (midpoint between A and B)
```

### Inputs

| Pin     | Type | Description                                  |
| ------- | ---- | -------------------------------------------- |
| **A/B** | Any  | Optional inline constant values for operands |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Filter Priority. Determines execution order when multiple blend operations are present in the blender.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Mode</strong> <code>EPCGExABBlendingType</code></summary>

Determines how Operand A and Operand B are combined.

| Option      | Description                                       |
| ----------- | ------------------------------------------------- |
| **Average** | Averages A and B (ignores weight)                 |
| **Weight**  | Weighted blend using the weight value             |
| **Min**     | Takes minimum of A and B (ignores weight)         |
| **Max**     | Takes maximum of A and B (ignores weight)         |
| **Copy**    | Copies A to output (ignores B and weight)         |
| **Sum**     | Adds A and B together                             |
| **Lerp**    | Linear interpolation between A and B using weight |
| **Mix**     | Additive blend: A + (B \* weight)                 |

Default: `Average`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand A</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read operand A value from. Supports point properties (e.g., $Position, $Rotation) and custom attributes.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Operand B</strong> <code>bool</code></summary>

Use a separate source for Operand B. If disabled, Operand A's input is reused for both operands.

Default: `false`

</details>

<details>

<summary><strong>Operand B</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Operand B attribute source. Only used if "Use Operand B" is enabled.

📋 _Visible when Use Operand B is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Mode</strong> <code>EPCGExBlendOpOutputMode</code></summary>

Choose where to output the result of the A/B blend.

| Option        | Description                                                             |
| ------------- | ----------------------------------------------------------------------- |
| **SameAsA**   | Writes result back to Operand A's attribute                             |
| **SameAsB**   | Writes result back to Operand B's attribute                             |
| **New**       | Creates a new persistent attribute                                      |
| **Transient** | Creates a temporary attribute (available only during current operation) |

Default: `SameAsA`

</details>

{% hint style="info" %}
## Transient Output: Chain Blend Operations Together

The **Transient** output mode is a powerful but easy-to-miss feature. Instead of writing to a permanent attribute, it creates a temporary value that:

* **Exists only during the blending pass** — it's automatically cleaned up afterward
* **Can be read by subsequent BlendOps** — operations with higher Priority values can reference it as an operand

This lets you build complex multi-step calculations without cluttering your data with intermediary attributes.

**Example: Normalized weighted blend**

```
Priority 0: Lerp(A, B, Weight) → Transient "TempBlend"
Priority 1: Divide(TempBlend, $Scale) → Final output
```

The first operation produces an intermediate result that the second operation consumes, then both the calculation and the temporary attribute disappear — leaving only your final value.

**Note**: Transient outputs may not work with all blending contexts. If a transient attribute isn't being found by a later operation, try using **New** output mode instead and cleaning up the attribute afterward.
{% endhint %}

<details>

<summary><strong>Output To</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Name of the output attribute when Output Mode is set to New or Transient.

📋 _Visible when Output Mode = New or Transient_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Reset Value Before Multi-Source Blend</strong> <code>bool</code></summary>

If enabled, when a node uses multiple sources for blending, the value is reset to 0 for specific blend modes (e.g., Sum) to avoid accounting for inherited values. This prevents accumulation of previous blend results.

Default: `true`

</details>

<details>

<summary><strong>Output Type</strong> <code>EPCGExOperandAuthority</code></summary>

Which type should be used for the output value. Only used if the output is not a point property.

| Option     | Description                                 |
| ---------- | ------------------------------------------- |
| **Auto**   | Automatically determines type from operands |
| **A**      | Uses Operand A's type                       |
| **B**      | Uses Operand B's type                       |
| **Custom** | Manually specify the output type            |

Default: `Auto`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Type</strong> <code>EPCGMetadataTypes</code></summary>

Manually specified output type when Output Type is set to Custom.

📋 _Visible when Output Type = Custom_

Default: `Double`

⚡ PCG Overridable

</details>

**Weighting Settings**

<details>

<summary><strong>Weight Input</strong> <code>EPCGExInputValueType</code></summary>

Type of weight source for weighted blend modes.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use a fixed weight value           |
| **Attribute** | Read weight from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight</strong> <code>double</code></summary>

Constant weight value used for blending. A weight of 0 favors Operand A entirely, 1 favors Operand B entirely, and 0.5 blends equally.

📋 _Visible when Weight Input = Constant_

Default: `0.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read weight value from. Weight is used per-point for blending.

📋 _Visible when Weight Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Local Curve</strong> <code>bool</code></summary>

Whether to use an in-editor curve or an external curve asset for remapping weight values.

Default: `false`

</details>

<details>

<summary><strong>Weight Curve</strong> <code>FRuntimeFloatCurve</code></summary>

In-editor curve that weight values are remapped over before blending. The X axis represents the input weight (0-1), and the Y axis represents the remapped output weight.

📋 _Visible when Use Local Curve is enabled_

</details>

<details>

<summary><strong>Weight Curve</strong> <code>TSoftObjectPtr</code></summary>

External curve asset that weight values are remapped over. Allows sharing curve configurations across multiple blend operations.

📋 _Visible when Use Local Curve is disabled_

Default: `WeightDistributionLinear` (built-in linear curve)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Curve Lookup</strong> <code>FPCGExCurveLookupDetails</code></summary>

Settings for how the weight curve is sampled (clamping, extrapolation behavior, etc.).

</details>

#### Inherited Settings

This node inherits common factory provider settings from its base class.

→ See [factory-provider-base.md](../../common-settings/shared-settings/factory-provider-base.md "mention") for shared provider configuration.

### Outputs

| Pin          | Type             | Description                                                                           |
| ------------ | ---------------- | ------------------------------------------------------------------------------------- |
| **Blending** | Blend Op Factory | The configured blend operation factory, ready to be used by an Attribute Blender node |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Core/PCGExBlendOpFactoryProvider.h)
