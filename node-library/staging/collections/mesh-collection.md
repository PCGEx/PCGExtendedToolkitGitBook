---
description: Curated collection of Static Mesh references.
icon: book
---

# Mesh Collection

### Overview

Mesh Collections are data assets that organize and manage lists of Static Mesh references. Each entry can reference either a specific Static Mesh or another Mesh Collection, allowing for hierarchical organization. Entries include configuration for material variants, component descriptors (ISM/SM settings), variations, weights, categories, and grammar rules.

### How It Works

1. **Entry Management**: Stores an array of Static Mesh references or sub-collections
2. **Material Variants**: Each mesh can define multiple material configurations with weighted selection
3. **Component Descriptors**: Configures rendering, physics, collision, and other mesh component properties per entry or globally
4. **Bounds Caching**: Pre-computes mesh bounds for efficient spatial queries and fitting operations

**Usage Notes**

* **Reusable Assets**: Collections are data assets that can be referenced by multiple nodes across different graphs
* **Hierarchical Structure**: Sub-collections allow organizing large mesh libraries into logical groups
* **Material Flexibility**: Supports single-slot or multi-slot material variants for dynamic material assignment
* **Descriptor Modes**: Choose between per-entry descriptors for fine control or global descriptors for consistency

### Settings

#### Global Mesh Configuration

<details>

<summary><strong>Global Descriptor Mode</strong> <code>EPCGExGlobalVariationRule</code></summary>

Controls whether mesh component descriptors are defined per entry or overridden globally for all entries in the collection.

| Option        | Description                                                     |
| ------------- | --------------------------------------------------------------- |
| **Per Entry** | Each entry uses its own descriptor settings                     |
| **Overrule**  | Global descriptor settings override all entry-level descriptors |

Default: `Per Entry`

</details>

<details>

<summary><strong>Global ISM Settings</strong> <code>FSoftISMComponentDescriptor</code></summary>

Component descriptor settings applied to all Instanced Static Mesh components when Global Descriptor Mode is set to Overrule. Controls rendering properties, collision, physics, and other ISM-specific settings.

Default: Empty (no overrides)

📋 _Used when Global Descriptor Mode = Overrule_

</details>

<details>

<summary><strong>Global SM Settings</strong> <code>FPCGExStaticMeshComponentDescriptor</code></summary>

Component descriptor settings applied to all Static Mesh components when Global Descriptor Mode is set to Overrule. Controls rendering properties, collision, physics, shadows, LOD, and other mesh component settings.

Default: Empty (no overrides)

📋 _Used when Global Descriptor Mode = Overrule_

</details>

#### Collection Entries

<details>

<summary><strong>Entries</strong> <code>TArray</code></summary>

Array of Static Mesh references or sub-collections that make up this collection. Each entry contains:

**Base Entry Properties:**

* **Weight**: Selection probability weight (higher values = more likely to be picked)
* **Category**: Optional category name for filtering entries
* **Is Sub Collection**: Toggle between mesh reference and sub-collection
* **Variation Mode**: Whether to use local or global variation settings
* **Variations**: Transform randomization settings (offset, rotation, scale)
* **Tags**: Set of tags for filtering and matching operations
* **Property Overrides**: Custom property values for this entry
* **Grammar Source**: Whether grammar details come from the asset or collection level
* **Asset Grammar**: Grammar symbol and scaling rules for this specific entry
* **Sub Grammar Mode**: How sub-collections handle grammar (inherit or override)
* **Collection Grammar**: Grammar settings when used as a sub-collection
* **Staging**: Asset staging configuration

**Mesh-Specific Properties:**

* **Static Mesh**: The Static Mesh to reference (visible when not a sub-collection)
* **Sub Collection**: Reference to another Mesh Collection (visible when Is Sub Collection is enabled)
* **Material Variants**: Material override mode (None, Single Slot, Multi Slots)
  * **Slot Index**: Material slot to override (Single Slot mode only)
  * **Variants**: Array of weighted material overrides for single or multiple slots
* **Descriptor Source**: Whether to use local or global descriptor settings
  * **ISM Settings**: Instanced Static Mesh component descriptor (local mode only)
  * **SM Settings**: Static Mesh component descriptor (local mode only)

Default: Empty array

</details>

#### Inherited Settings

This collection type inherits common asset collection functionality from its base class.

→ See [asset-collection-base.md](asset-collection-base.md "mention") documentation for: Global variations, property schemas, grammar settings, and collection-level configuration

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Collections/PCGExMeshCollection.h)
