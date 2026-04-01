---
description: Creates a single partition rule to be used with the Partition by Values node.
icon: circle-dashed
---

# Partition Rule

### Overview

This sub-node defines a partition rule that specifies how points should be grouped based on attribute values. The rule transforms attribute values into partition keys using configurable scaling, filtering, and clamping operations. Multiple partition rules can be connected to create complex multi-attribute partitioning schemes.

### How It Works

1. **Read Attribute**: Reads the specified attribute value from each point.
2. **Transform Value**: Applies upscale, offset, and filter mode to compute a partition key.
3. **Clamp/Modify**: Optionally clamps, inverts, or takes absolute value of the key.
4. **Output Key**: Produces an integer key that determines which bucket the point belongs to.

**Usage Notes**

* **Filter Modes**: Floor, Ceil, Round, and Truncate determine how floating-point values become integer keys.
* **Modulo**: Use modulo to wrap keys into a limited range of buckets.
* **Key Writing**: Optionally write the computed partition key back to an attribute or tag.

### Settings

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read values from for partitioning.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Enabled</strong> <code>bool</code></summary>

Whether this partition rule is active.

Default: `true`

</details>

<details>

<summary><strong>Filter Size</strong> <code>double</code></summary>

Divides the attribute value by this amount before computing the key. Larger values create coarser partitions.

Default: `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Upscale</strong> <code>double</code></summary>

Multiplies the attribute value before filtering. Use to amplify small value differences.

Default: `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Added to the value before computing the key. Use to shift partition boundaries.

Default: `0.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Filter Mode</strong> <code>EPCGExPartitionFilterMode</code></summary>

How to convert floating-point values to integer partition keys.

| Option       | Description                   |
| ------------ | ----------------------------- |
| **Floor**    | Round down to nearest integer |
| **Ceil**     | Round up to nearest integer   |
| **Round**    | Round to nearest integer      |
| **Truncate** | Remove decimal portion        |

Default: `Floor`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Modulo Value</strong> <code>int32</code></summary>

When non-zero, wraps keys using modulo operation. Creates cyclical partition buckets.

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Clamp Key</strong> <code>bool</code></summary>

When enabled, clamps the partition key to a min/max range.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Key Clamp Min</strong> <code>int64</code></summary>

Minimum allowed key value when clamping is enabled.

Default: `0`

📋 _Visible when Clamp Key is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Key Clamp Max</strong> <code>int64</code></summary>

Maximum allowed key value when clamping is enabled.

Default: `100`

📋 _Visible when Clamp Key is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Key</strong> <code>bool</code></summary>

When enabled, negates the partition key.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute Key</strong> <code>bool</code></summary>

When enabled, uses absolute value of the key, merging positive and negative values.

Default: `false`

⚡ PCG Overridable

</details>

**Output Settings**

<details>

<summary><strong>Write Key</strong> <code>bool</code></summary>

When enabled, writes the computed partition key to an attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Key Attribute Name</strong> <code>FName</code></summary>

Attribute name to write the partition key to.

Default: `@Data.PartitionKey`

📋 _Visible when Write Key is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Partition Index As Key</strong> <code>bool</code></summary>

When enabled, writes the partition index instead of the computed key.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Tag</strong> <code>bool</code></summary>

When enabled, adds a tag with the partition key to output data.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Prefix Name</strong> <code>FName</code></summary>

Prefix for the partition tag.

Default: `Partition`

📋 _Visible when Write Tag is enabled_

⚡ PCG Overridable

</details>

### Outputs

| Pin               | Type   | Description                                             |
| ----------------- | ------ | ------------------------------------------------------- |
| **PartitionRule** | Params | Partition rule factory for use with Partition by Values |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/Partition/PCGExModularPartitionByValues.h)
