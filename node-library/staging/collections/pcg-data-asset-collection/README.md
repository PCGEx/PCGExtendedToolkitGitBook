---
description: Curated collection of PCG Data Asset references with optional level export.
icon: book
---

# PCG Data Asset Collection

### Overview

PCG Data Asset Collections are data assets that organize and manage lists of `UPCGDataAsset` references. Each entry can reference an existing PCG Data Asset directly, point to a `UWorld` level that gets exported into an embedded data asset at staging time, or reference another PCG Data Asset Collection as a sub-collection. When entries use the Level source mode, a configurable Level Data Exporter handles the conversion.

### How It Works

1. **Entry Management**: Stores an array of PCG Data Asset references, level references, or sub-collections
2. **Dual Source Modes**: Each entry can source from an existing `UPCGDataAsset` or export from a `UWorld` level
3. **Level Export**: Level-sourced entries use the collection's Level Exporter (or a built-in default) to convert world actors into embedded PCG Data Assets with point data, metadata, and optionally embedded Mesh/Actor collections
4. **Bounds Caching**: Pre-computes combined bounds from all spatial data in each referenced or exported asset

**Usage Notes**

* **Reusable Assets**: Collections are data assets that can be referenced by multiple nodes across different graphs
* **Hierarchical Structure**: Sub-collections allow organizing large asset libraries into logical groups
* **Level Exporter**: The collection's Level Exporter instance is shared across all level-sourced entries. Customize it to control filtering, classification, and collection generation behavior.
* **Embedded Assets**: Level-sourced entries create embedded `UPCGDataAsset` objects outered to the collection. These are serialized with the collection and rebuilt when staging is updated.
* **Browser Drag-and-Drop**: Supports adding both `UWorld` and `UPCGDataAsset` files from the Content Browser — levels are automatically configured with the Level source mode

### Settings

#### Level Export

<details>

<summary><strong>Level Exporter</strong> <code>UPCGExLevelDataExporter</code> (Instanced)</summary>

The exporter used to convert level-sourced entries into embedded PCG Data Assets during staging. Instanced inline, so custom exporters expose their own settings directly in the collection's details panel.

If unset, a default exporter is used.

→ See Default Level Data Exporter for the built-in exporter and its settings.

</details>

#### Collection Entries

<details>

<summary><strong>Entries</strong> <code>TArray&#x3C;FPCGExPCGDataAssetCollectionEntry></code></summary>

Array of PCG Data Asset references, level references, or sub-collections that make up this collection. Each entry contains:

**Base Entry Properties:**

* **Weight**: Selection probability weight (higher values = more likely to be picked)
* **Category**: Optional category name for filtering entries
* **Is Sub Collection**: Toggle between asset reference and sub-collection
* **Variation Mode**: Whether to use local or global variation settings
* **Variations**: Transform randomization settings (offset, rotation, scale)
* **Tags**: Set of tags for filtering and matching operations
* **Property Overrides**: Custom property values for this entry
* **Grammar Source**: Whether grammar details come from the asset or collection level
* **Asset Grammar**: Grammar symbol and scaling rules for this specific entry
* **Sub Grammar Mode**: How sub-collections handle grammar (inherit or override)
* **Collection Grammar**: Grammar settings when used as a sub-collection
* **Staging**: Asset staging configuration

**PCG Data Asset-Specific Properties:**

* **Source**: `Data Asset` or `Level` — determines how the entry is resolved (visible when not a sub-collection)
* **Data Asset**: Reference to an existing `UPCGDataAsset` (visible when Source = Data Asset)
* **Level**: Reference to a UWorld level asset (visible when Source = Level)
* **Sub Collection**: Reference to another PCG Data Asset Collection (visible when Is Sub Collection is enabled)

Default: Empty array

</details>

#### Inherited Settings

This collection type inherits common asset collection functionality from its base class.

→ See [asset-collection-base.md](../asset-collection-base.md "mention") documentation for: Global variations, property schemas, grammar settings, and collection-level configuration

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Collections/PCGExPCGDataAssetCollection.h)
