---
description: Curated collection of Level (UWorld) references.
icon: book
---

# Level Collection

### Overview

Level Collections are data assets that organize and manage lists of Level references. Each entry can reference either a specific UWorld level asset or another Level Collection, allowing for hierarchical organization. Entries include configuration for bounds filtering, variations, weights, categories, grammar rules, and property staging.

### How It Works

1. **Entry Management**: Stores an array of Level references or sub-collections
2. **Bounds Computation**: Loads the level in-editor and computes combined bounds from the spatial actors it contains
3. **Bounds Filtering**: Actors contributing to bounds can be filtered by tag, class, and collision settings
4. **Weighting**: Each entry has a weight value affecting its selection probability in sampling operations

**Usage Notes**

* **Reusable Assets**: Collections are data assets that can be referenced by multiple nodes across different graphs
* **Hierarchical Structure**: Sub-collections allow organizing large level libraries into logical groups
* **Editor-Only Bounds**: Bounds computation requires loading the level package, which only happens in-editor. Runtime contexts do not recompute bounds.
* **Actor Filtering**: Infrastructure actors (LevelScriptActor, Info, Brush), hidden actors, and editor-only actors are automatically excluded from bounds computation before any user-defined filters are applied

### Settings

#### Collection Entries

<details>

<summary><strong>Entries</strong> <code>TArray&#x3C;FPCGExLevelCollectionEntry></code></summary>

Array of Level references or sub-collections that make up this collection. Each entry contains:

**Base Entry Properties:**

* **Weight**: Selection probability weight (higher values = more likely to be picked)
* **Category**: Optional category name for filtering entries
* **Is Sub Collection**: Toggle between Level reference and sub-collection
* **Variation Mode**: Whether to use local or global variation settings
* **Variations**: Transform randomization settings (offset, rotation, scale)
* **Tags**: Set of tags for filtering and matching operations
* **Property Overrides**: Custom property values for this entry
* **Grammar Source**: Whether grammar details come from the asset or collection level
* **Asset Grammar**: Grammar symbol and scaling rules for this specific entry
* **Sub Grammar Mode**: How sub-collections handle grammar (inherit or override)
* **Collection Grammar**: Grammar settings when used as a sub-collection
* **Staging**: Asset staging configuration

**Level-Specific Properties:**

* **Level**: The UWorld level asset to reference (visible when not a sub-collection)
* **Sub Collection**: Reference to another Level Collection (visible when Is Sub Collection is enabled)

**Bounds Filtering** (visible when not a sub-collection):

* **Bounds Include Tags**: Only actors with at least one of these tags contribute to bounds. Empty = all actors.
* **Bounds Exclude Tags**: Actors with any of these tags are excluded from bounds computation
* **Bounds Include Classes**: Only actors of these classes (or subclasses) contribute to bounds. Empty = all classes.
* **Bounds Exclude Classes**: Actors of these classes (or subclasses) are excluded from bounds
* **Only Colliding Components**: If enabled, only primitive components with collision contribute to bounds

Default: Empty array

</details>

#### Inherited Settings

This collection type inherits common asset collection functionality from its base class.

→ See [asset-collection-base.md](asset-collection-base.md "mention") documentation for: Global variations, property schemas, grammar settings, and collection-level configuration

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Collections/PCGExLevelCollection.h)
