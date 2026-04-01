---
description: Foundation for all asset collection types.
icon: gear-complex
---

# Asset Collection Base

### Overview

Asset Collection is the abstract base class providing core functionality for organizing and managing curated lists of assets. It implements weighted random selection, category-based grouping, transform variations, grammar systems, and property schema management. Specific collection types (Actor, Mesh, PCG Data Asset) inherit from this base to add type-specific features.

### How It Works

1. **Entry Organization**: Stores entries with weights, categories, and tags for flexible selection
2. **Selection Cache**: Pre-computes weighted picking order for efficient runtime access
3. **Property System**: Defines collection-level default properties that entries can override
4. **Grammar Integration**: Supports grammar-based procedural generation rules with size and scaling configuration
5. **Variation Management**: Provides global or per-entry transform randomization settings

**Usage Notes**

* **Abstract Base**: This class is not used directly - specific collection types inherit its functionality
* **Cache Building**: Collections automatically build an optimized cache for fast weighted random selection
* **Property Inheritance**: Entries inherit collection-level properties unless they provide overrides
* **Circular Dependencies**: The system detects and prevents circular references in sub-collection hierarchies

### Settings

#### Collection Metadata

<details>

<summary><strong>Notes</strong> <code>FString</code></summary>

Editor-only field for documenting the purpose and contents of this collection.

Default: Empty

📋 _Editor-only_

</details>

<details>

<summary><strong>Collection Tags</strong> <code>TSet</code></summary>

Tags applied to the collection itself, used for filtering and identification when collections are referenced by nodes.

Default: Empty set

</details>

<details>

<summary><strong>Auto Rebuild Staging</strong> <code>bool</code></summary>

Automatically rebuild staging data (bounds, paths, sockets) when the collection is modified in the editor.

Default: `true`

📋 _Editor-only, Advanced_

</details>

#### Global Variations

<details>

<summary><strong>Global Variation Mode</strong> <code>EPCGExGlobalVariationRule</code></summary>

Controls whether transform variations are defined per entry or globally for all entries.

| Option        | Description                                                   |
| ------------- | ------------------------------------------------------------- |
| **Per Entry** | Each entry uses its own variation settings                    |
| **Overrule**  | Global variation settings override all entry-level variations |

Default: `Per Entry`

</details>

<details>

<summary><strong>Global Variations</strong> <code>FPCGExFittingVariations</code></summary>

Global transform randomization settings applied to all entries when Global Variation Mode is set to Overrule. Controls offset, rotation, and scale variation ranges with optional snapping.

Default: No variations

📋 _Used when Global Variation Mode = Overrule_

</details>

#### Grammar Settings

<details>

<summary><strong>Global Grammar Mode</strong> <code>EPCGExGlobalVariationRule</code></summary>

Controls whether grammar details are defined per entry or globally for all entries.

| Option        | Description                                              |
| ------------- | -------------------------------------------------------- |
| **Per Entry** | Each entry uses its own grammar settings                 |
| **Overrule**  | Global grammar settings override all entry-level grammar |

Default: `Per Entry`

</details>

<details>

<summary><strong>Global Asset Grammar</strong> <code>FPCGExAssetGrammarDetails</code></summary>

Global grammar configuration for asset-level rules when Global Grammar Mode is set to Overrule. Defines grammar symbols, scale modes, and size references for procedural generation systems.

Default: Symbol "N/A"

📋 _Used when Global Grammar Mode = Overrule_

</details>

<details>

<summary><strong>Collection Grammar</strong> <code>FPCGExCollectionGrammarDetails</code></summary>

Grammar details for this collection when it is used as a sub-collection within another collection. Defines how the collection should be interpreted by grammar-based generation systems.

Default: Empty

</details>

#### Validation

<details>

<summary><strong>Do Not Ignore Invalid Entries</strong> <code>bool</code></summary>

By default, entries with zero weight or missing asset references are excluded from selection. Enable this to include all entries regardless of validity.

Default: `false`

</details>

#### Properties

<details>

<summary><strong>Collection Properties</strong> <code>FPCGExPropertySchemaCollection</code></summary>

Collection-level properties with default values. These define custom attributes that can be written to points when assets are selected. Individual entries can override these values to provide entry-specific property values.

Default: Empty schema

</details>

<details>

<summary><strong>Property Registry</strong> <code>TArray</code></summary>

Read-only registry automatically built from Collection Properties. Shows available property names, types, and whether they support output to point data.

Default: Auto-generated from schema

📋 _Read-only, auto-updated_

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Core/PCGExAssetCollection.h)
