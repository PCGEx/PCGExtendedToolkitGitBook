---
description: Hoist element values to tags or data domain.
icon: circle
---

# Hoist Attributes

### Overview

This node extracts attribute values from points and promotes them to collection-level metadata. Values can be converted to data tags, written to the @Data domain, or output as a new attribute set. The node supports various resolution modes for matching source entries to target collections, and can optionally prefix values with the attribute name.

### How It Works

1. **Select Source**: Determines which entries to read based on resolution mode.
2. **Read Attributes**: Extracts values from specified attributes on selected points.
3. **Format Output**: Optionally prefixes values with attribute names.
4. **Apply Action**: Adds as tags, writes to @Data domain, or creates attribute set.

**Usage Notes**

* **Resolution Modes**: "Self" uses the collection's own entries, "Entry to Collection" uses a separate source, "Collection to Collection" pairs sources 1:1.
* **Selection**: When not using all entries, specify which entry to use (first, last, random, or via pickers).
* **Prefix Option**: Enable prefixing to create structured tags like "BiomeType:Forest" instead of just "Forest".
* **Comma Separation**: Use the comma-separated field for easy override of attribute selectors.

### Behavior

```
Hoist to Tags:

Input Collection with point attributes:
   Point 0: BiomeType = "Forest", Difficulty = 3
   Point 1: BiomeType = "Desert", Difficulty = 5

Selection = First Entry, Prefix = true:
   Tags added: ["BiomeType:Forest", "Difficulty:3"]

Selection = First Entry, Prefix = false:
   Tags added: ["Forest", "3"]
```

### Inputs

| Pin             | Type   | Description                                                             |
| --------------- | ------ | ----------------------------------------------------------------------- |
| **In**          | Points | Input point collections                                                 |
| **Tags Source** | Points | Source data for tag values (when using Entry/Collection resolution)     |
| **Pickers**     | Params | Picker sub-nodes for index selection (when using Picker selection mode) |

### Settings

<details>

<summary><strong>Action</strong> <code>EPCGExAttributeToTagsAction</code></summary>

Determines how attribute values are output.

| Option                     | Description                   |
| -------------------------- | ----------------------------- |
| **Hoist to Tags**          | Add values as data tags       |
| **Hoist to Attribute Set** | Output to a new attribute set |
| **Hoist to @Data**         | Write values to @Data domain  |

Default: `Hoist to Tags`

</details>

<details>

<summary><strong>Resolution</strong> <code>EPCGExAttributeToTagsResolution</code></summary>

How to match source entries to target collections.

| Option                       | Description                                         |
| ---------------------------- | --------------------------------------------------- |
| **Self**                     | Use entries from the collection itself              |
| **Entry to Collection**      | Match source entries to each input collection       |
| **Collection to Collection** | Match source collections 1:1 with input collections |

Default: `Self`

</details>

<details>

<summary><strong>Selection</strong> <code>EPCGExCollectionEntrySelection</code></summary>

Which entry to use when a single value is needed.

| Option             | Description                                    |
| ------------------ | ---------------------------------------------- |
| **First Entry**    | Use the first entry in the matching collection |
| **Last Entry**     | Use the last entry in the matching collection  |
| **Random Entry**   | Use a random entry in the matching collection  |
| **Picker**         | Use pickers to select indices for tagging      |
| **Picker (First)** | Use the first valid index from pickers         |
| **Picker (Last)**  | Use the last valid index from pickers          |

Default: `First Entry`

📋 _Visible when Resolution is not Entry to Collection_

</details>

<details>

<summary><strong>Prefix With Attribute Name</strong> <code>bool</code></summary>

When enabled, prefixes the attribute value with the attribute name (e.g., "AttrName:Value").

Default: `true`

</details>

<details>

<summary><strong>Attributes</strong> <code>TArray&#x3C;FPCGAttributePropertyInputSelector></code></summary>

List of attribute selectors whose values will be hoisted.

</details>

<details>

<summary><strong>Comma Separated Attribute Selectors</strong> <code>FString</code></summary>

A comma-separated list of attribute selectors for easy overrides. Appended to the Attributes array.

⚡ PCG Overridable

</details>

#### Warning Settings

<details>

<summary><strong>Quiet Too Many Collections Warning</strong> <code>bool</code></summary>

Suppress warning when source has more collections than target.

Default: `false`

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin      | Type   | Description                                           |
| -------- | ------ | ----------------------------------------------------- |
| **Out**  | Points | Input collections with added tags or @Data attributes |
| **Tags** | Params | Attribute set output (when Action = Attribute Set)    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExAttributesToTags.h)
