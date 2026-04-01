---
description: Curated collection of Actor class references.
icon: book
---

# Actor Collection

### Overview

Actor Collections are data assets that organize and manage lists of Actor class references. Each entry can reference either a specific Actor blueprint class or another Actor Collection, allowing for hierarchical organization. Entries include configuration for variations, weights, categories, grammar rules, and property staging.

### How It Works

1. **Entry Management**: Stores an array of Actor class references or sub-collections
2. **Weighting**: Each entry has a weight value affecting its selection probability in sampling operations
3. **Categorization**: Entries can be grouped using category tags for filtered access
4. **Bounds Caching**: Pre-computes Actor bounds for efficient spatial queries and fitting operations

**Usage Notes**

* **Reusable Assets**: Collections are data assets that can be referenced by multiple nodes across different graphs
* **Hierarchical Structure**: Sub-collections allow organizing large Actor libraries into logical groups
* **Bounds Calculation**: Actor bounds are cached to optimize operations like socket fitting and overlap testing
* **Property Staging**: Each entry can define custom properties to be written to spawned points

### Settings

#### Collection Entries

<details>

<summary><strong>Entries</strong> <code>TArray</code></summary>

Array of Actor references or sub-collections that make up this collection. Each entry contains:

**Base Entry Properties:**

* **Weight**: Selection probability weight (higher values = more likely to be picked)
* **Category**: Optional category name for filtering entries
* **Is Sub Collection**: Toggle between Actor reference and sub-collection
* **Variation Mode**: Whether to use local or global variation settings
* **Variations**: Transform randomization settings (offset, rotation, scale)
* **Tags**: Set of tags for filtering and matching operations
* **Property Overrides**: Custom property values for this entry
* **Grammar Source**: Whether grammar details come from the asset or collection level
* **Asset Grammar**: Grammar symbol and scaling rules for this specific entry
* **Sub Grammar Mode**: How sub-collections handle grammar (inherit or override)
* **Collection Grammar**: Grammar settings when used as a sub-collection
* **Staging**: Asset staging configuration

**Actor-Specific Properties:**

* **Actor**: The Actor class to reference (visible when not a sub-collection)
* **Sub Collection**: Reference to another Actor Collection (visible when Is Sub Collection is enabled)
* **Only Colliding Components**: Include only components with collision in bounds calculation
* **Include From Child Actors**: Whether to include child actor bounds in calculations

Default: Empty array

</details>

#### Inherited Settings

This collection type inherits common asset collection functionality from its base class.

→ See [asset-collection-base.md](asset-collection-base.md "mention") documentation for: Global variations, property schemas, grammar settings, and collection-level configuration

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Collections/PCGExActorCollection.h)
