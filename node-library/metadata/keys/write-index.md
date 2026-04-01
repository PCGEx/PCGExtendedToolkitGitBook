---
description: Multi-purpose index-related node.
icon: circle
---

# Write Index

### Overview

This node writes index information to point attributes and collection metadata. It can output the point's index within its collection (optionally normalized or inverted), the collection's index among all inputs, and the total number of entries in each collection. These values are essential for procedural operations that need to reference position within sequences or across collections.

### How It Works

1. **Calculate Indices**: Determines each point's index and collection-level metadata.
2. **Apply Transforms**: Optionally inverts or normalizes index values.
3. **Write Values**: Outputs to point attributes and/or @Data domain tags.

**Usage Notes**

* **Point Index**: Written per-point; use "One Minus" for reverse ordering or "Normalized" for 0-1 range.
* **Collection Index**: Identifies which collection a point belongs to; can write to @Data tags.
* **Num Entries**: Total point count per collection; useful for normalization calculations.
* **Tag Output**: Collection-level values can be written as tags for use in downstream nodes.

### Behavior

**Index Writing:**

```
Collection A (3 points):
   Point 0: Index = 0  (Normalized: 0.0,  OneMinus: 2)
   Point 1: Index = 1  (Normalized: 0.5,  OneMinus: 1)
   Point 2: Index = 2  (Normalized: 1.0,  OneMinus: 0)
   @Data.CollectionIndex = 0
   @Data.NumEntries = 3

Collection B (2 points):
   Point 0: Index = 0  (Normalized: 0.0,  OneMinus: 1)
   Point 1: Index = 1  (Normalized: 1.0,  OneMinus: 0)
   @Data.CollectionIndex = 1
   @Data.NumEntries = 2
```

### Inputs

| Pin    | Type   | Description             |
| ------ | ------ | ----------------------- |
| **In** | Points | Input point collections |

### Settings

#### Point Index Settings

<details>

<summary><strong>Output Point Index</strong> <code>bool</code></summary>

When enabled, writes the point's index within its collection to an attribute.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Attribute Name</strong> <code>FName</code></summary>

The attribute name to write the point index to.

Default: `CurrentIndex`

📋 _Visible when Output Point Index is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>One Minus</strong> <code>bool</code></summary>

Inverts the index (MaxIndex - Index) before writing. Useful for reverse ordering.

Default: `false`

📋 _Visible when Output Point Index is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normalized</strong> <code>bool</code></summary>

Writes the index as a normalized value (0.0 to 1.0 range based on collection size).

Default: `false`

📋 _Visible when Output Point Index is enabled_

⚡ PCG Overridable

</details>

#### Collection Index Settings

<details>

<summary><strong>Output Collection Index</strong> <code>bool</code></summary>

When enabled, writes the collection's index among all input collections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Collection Index Attribute Name</strong> <code>FName</code></summary>

The attribute or tag name to write the collection index to.

Default: `@Data.CollectionIndex`

📋 _Visible when Output Collection Index is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Type</strong> <code>EPCGExNumericOutput</code></summary>

Output data type for the collection index attribute.

| Option     | Description            |
| ---------- | ---------------------- |
| **Int32**  | 32-bit integer         |
| **Int64**  | 64-bit integer         |
| **Float**  | Single precision float |
| **Double** | Double precision float |

Default: `Int32`

📋 _Visible when Output Collection Index is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output to Tags</strong> <code>bool</code></summary>

When enabled, outputs the collection index as a tag in the @Data domain.

Default: `false`

📋 _Visible when Output Collection Index is enabled_

⚡ PCG Overridable

</details>

#### Num Entries Settings

<details>

<summary><strong>Output Collection Num Entries</strong> <code>bool</code></summary>

When enabled, writes the total number of points in each collection.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Num Entries Attribute Name</strong> <code>FName</code></summary>

The attribute or tag name to write the entry count to.

Default: `@Data.NumEntries`

📋 _Visible when Output Collection Num Entries is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Type</strong> <code>EPCGExNumericOutput</code></summary>

Output data type for the num entries attribute.

| Option     | Description            |
| ---------- | ---------------------- |
| **Int32**  | 32-bit integer         |
| **Int64**  | 64-bit integer         |
| **Float**  | Single precision float |
| **Double** | Double precision float |

Default: `Int32`

📋 _Visible when Output Collection Num Entries is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normalized</strong> <code>bool</code></summary>

Writes the entry count as a normalized value relative to the maximum across all collections.

Default: `false`

📋 _Visible when Output Collection Num Entries is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output to Tags</strong> <code>bool</code></summary>

When enabled, outputs the entry count as a tag in the @Data domain.

Default: `false`

📋 _Visible when Output Collection Num Entries is enabled_

⚡ PCG Overridable

</details>

#### General Settings

<details>

<summary><strong>Allow Interpolation</strong> <code>bool</code></summary>

Whether the created attributes allow interpolation during blending operations.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                                   |
| ------- | ------ | ------------------------------------------------------------- |
| **Out** | Points | Points with index attributes and optional collection metadata |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExWriteIndex.h)
