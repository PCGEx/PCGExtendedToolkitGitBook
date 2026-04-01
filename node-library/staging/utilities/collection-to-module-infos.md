---
description: Convert an asset collection to grammar-friendly module infos.
icon: circle
---

# Collection to Module Infos

### Overview

This node extracts grammar-related data from an Asset Collection and outputs it as module infos compatible with PCG's grammar subdivision system. Each collection entry's grammar configuration (symbol, size, scalability, debug color) is written to an attribute set suitable for use with grammar nodes.

### How It Works

1. **Collection Loading**: Loads and validates the specified Asset Collection
2. **Grammar Extraction**: Extracts grammar details from each entry (symbol, size, scalable flag, debug color)
3. **Entry Indexing**: Assigns unique entry identifiers for later retrieval by Grammar Staging nodes
4. **Attribute Writing**: Creates one point per entry with grammar properties as attributes
5. **Filtering**: Optionally removes duplicates, empty symbols, and invalid entries

**Usage Notes**

* **Grammar Integration**: Designed specifically for use with PCG grammar subdivision and PCGEx Grammar Staging
* **Symbol Uniqueness**: Duplicate symbols can be allowed or filtered based on use case requirements
* **Entry Reference**: The Entry attribute stores collection/index references critical for Grammar Staging retrieval
* **Empty Symbols**: Entries with "None" symbols are typically used as placeholders and can be filtered

### Outputs

| Pin              | Type          | Description                              |
| ---------------- | ------------- | ---------------------------------------- |
| **Module Infos** | Attribute Set | Grammar module information as attributes |
| **Labels**       | Labels        | Additional labeled data                  |

### Settings

#### Collection Configuration

<details>

<summary><strong>Asset Collection</strong> <code>TSoftObjectPtr</code></summary>

The Asset Collection to extract module infos from. Grammar details must be configured in the collection entries.

Default: None

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Duplicates</strong> <code>bool</code></summary>

Whether to allow duplicate entries in the output. Duplicates are identified by matching grammar symbols.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Skip Empty Symbol</strong> <code>bool</code></summary>

Filter out entries where the grammar symbol is "None". Empty symbols are often used as placeholders that shouldn't appear in grammar generation.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Invalid And Empty</strong> <code>bool</code></summary>

Remove invalid entries (missing asset references, zero weight) and empty sub-collections from the output.

Default: `true`

⚡ PCG Overridable

</details>

#### Output Attributes

<details>

<summary><strong>Symbol</strong> <code>FName</code></summary>

Name of the attribute to write the grammar symbol to. The symbol identifies the module type in grammar rules.

Default: `Symbol`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Size</strong> <code>FName</code></summary>

Name of the attribute to write the module size to. Size determines the module's spatial extent in grammar-based placement, calculated from asset bounds and scale mode settings.

Default: `Size`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scalable</strong> <code>FName</code></summary>

Name of the attribute to write the scalable flag to. Indicates whether the module can be scaled to fit available space during grammar subdivision.

Default: `Scalable`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Debug Color</strong> <code>FName</code></summary>

Name of the attribute to write the debug visualization color to. Used for visual debugging of grammar generation.

Default: `DebugColor`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Entry</strong> <code>FName</code></summary>

Name of the attribute to write the entry index to. This serializes the collection ID and entry index, which is critical for Grammar Staging nodes to retrieve the corresponding asset.

Default: Internal entry index tag

⚡ PCG Overridable

📋 _Attribute name is managed internally_

</details>

<details>

<summary><strong>Category</strong> <code>FName</code></summary>

Name of the attribute to write the entry's category value to.

Default: `Category`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExCollectionToModuleInfos.h)
