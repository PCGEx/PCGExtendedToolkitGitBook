---
description: Creates bulk blend operations from monolithic blending settings.
icon: circle-dashed
---

# BlendOp : All

#### Overview

This provider node creates blend operations for all attributes at once using a single configuration block. Instead of wiring individual [blendop.md](blendop.md "mention") sub-nodes for each attribute, you configure a default blend mode and optionally override specific properties or attributes. The node generates one blend operation per participating attribute automatically.&#x20;

> Think of this as a [blending-details-monolithic.md](../../common-settings/blending-details-monolithic.md "mention") in node form.

#### How It Works

1. **Configure Defaults**: Set a default blend mode applied to all participating attributes.
2. **Filter Attributes**: Optionally include or exclude specific attributes from blending.
3. **Apply Overrides**: Assign different blend modes to individual point properties or named attributes.
4. **Generate Operations**: The factory creates one internal blend operation per attribute, each configured according to the defaults and overrides.

**Usage Notes**

* **Bulk vs Individual**: This node replaces multiple individual [blendop.md](blendop.md "mention") sub-nodes when you want the same (or similar) treatment across many attributes. Use individual [blendop.md](blendop.md "mention") when you need per-attribute operand control, custom weights, or transient chaining.
* **Preconfigured Variants**: The node offers preconfigured presets for each blend mode (Average, Weight, Min, Max, etc.) for quick setup.

{% hint style="warning" %}
## **Supersede Behavior**

When both monolithic and individual [blendop.md](blendop.md "mention") target the same attribute, individual [blendop.md](blendop.md "mention") take priority and the monolithic operation for that attribute is skipped.
{% endhint %}

#### Behavior

```
Monolithic Blending (Default: Average, Override: Density = Max)

Source Attributes: Color, Density, Score, Health

Generated Operations:
  Color   → Average (default)
  Density → Max     (override)
  Score   → Average (default)
  Health  → Average (default)

Equivalent to wiring 4 individual BlendOp nodes,
but configured in a single settings block.
```

#### Inputs

| Pin     | Type | Description                                  |
| ------- | ---- | -------------------------------------------- |
| **A/B** | Any  | Optional inline constant values for operands |

#### Settings

**Node-Specific Settings**

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Filter Priority. Determines execution order when multiple blend operations are present.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

Monolithic blending configuration. Controls which attributes participate, the default blend mode, and per-property/per-attribute overrides.

⚡ PCG Overridable

</details>

> See  [blending-details-monolithic.md](../../common-settings/blending-details-monolithic.md "mention") for: Blending Filter, Filtered Attributes, Default Blending, Properties Overrides, Attributes Overrides.

**Inherited Settings**

This node inherits common factory provider settings from its base class.

> See [factory-provider-base.md](../../common-settings/shared-settings/factory-provider-base.md "mention") for shared provider configuration.

#### Outputs

| Pin          | Type             | Description                                                                |
| ------------ | ---------------- | -------------------------------------------------------------------------- |
| **Blending** | Blend Op Factory | The configured blend operation factory, ready to be used by consumer nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Core/PCGExBlendOpMonolithicFactoryProvider.h)
