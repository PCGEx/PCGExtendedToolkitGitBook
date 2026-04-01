---
description: Exports level actors as typed point data with optional collection generation.
icon: function
---

# Level Data Exporter (Default)

### Overview

The Default Level Data Exporter reads all qualifying actors from a UWorld level and produces typed point data organized by classification: Mesh (actors with static mesh components) and Actor (everything else). Each point is placed at its source actor's transform with computed bounds. When collection generation is enabled, the exporter also builds embedded Mesh and Actor collections with entry hashes on each point, ready for downstream staging workflows.

### How It Works

1. **Filter Actors**: Iterates actors in the level's persistent level, skipping hidden, editor-only, and infrastructure actors (LevelScriptActor, Info, Brush). Applies user-defined tag and class include/exclude filters.
2. **Classify**: Each surviving actor is classified as Mesh (has a `UStaticMeshComponent` with a valid mesh), Actor (no static mesh), or Skip. ISM instances are also extracted as individual mesh points.
3. **Create Point Data**: Builds separate point datasets for "Meshes" and "Actors", each with transforms, bounds, and metadata attributes (`ActorName`, plus `Mesh` or `ActorClass` paths).
4. **Generate Collections** (optional): When enabled, builds embedded `UPCGExMeshCollection` and `UPCGExActorCollection` assets, encodes collection entry hashes on each point, and outputs a Collection Map for downstream staging nodes.

**Usage Notes**

* **Editor-Only Bounds**: Bounds computation loads the level package in-editor. Runtime export is not supported.
* **ISM Expansion**: Instanced Static Mesh components on any classified actor are expanded into individual points, one per instance.
* **Material Variants**: When collection generation and material capture are both enabled, unique material override combinations are tracked as separate variants on mesh collection entries.
* **Property Deltas**: When enabled, per-instance property differences from the class default object (CDO) are serialized and stored on actor collection entries, allowing reconstruction of per-instance overrides.
* **Extensible**: Subclass and override `ClassifyActor` to customize how actors are categorized, or `OnExportComplete` for post-export processing.

### Settings

#### Filtering

<details>

<summary><strong>Include Tags</strong> <code>TArray&#x3C;FName></code></summary>

If non-empty, only actors with at least one of these tags are exported. Empty means all actors pass.

Default: Empty

</details>

<details>

<summary><strong>Exclude Tags</strong> <code>TArray&#x3C;FName></code></summary>

Actors with any of these tags are excluded from export.

Default: Empty

</details>

<details>

<summary><strong>Include Classes</strong> <code>TArray&#x3C;TSoftClassPtr&#x3C;AActor>></code></summary>

If non-empty, only actors of these classes (or subclasses) are exported. Empty means all classes pass.

Default: Empty

</details>

<details>

<summary><strong>Exclude Classes</strong> <code>TArray&#x3C;TSoftClassPtr&#x3C;AActor>></code></summary>

Actors of these classes (or subclasses) are excluded from export.

Default: Empty

</details>

#### Collection Generation

<details>

<summary><strong>Generate Collections</strong> <code>bool</code></summary>

When enabled, the exporter builds embedded Mesh and Actor collections from the exported data. Raw metadata attributes (`Mesh`, `ActorClass`) are replaced with collection entry hashes (`PCGEx/CollectionEntry`), and a Collection Map is output for downstream staging.

Default: `false`

</details>

<details>

<summary><strong>Capture Material Overrides</strong> <code>bool</code></summary>

When enabled, material overrides from source mesh components are captured and stored as material variants on mesh collection entries. Unique material combinations become separate weighted variants.

Default: `true`

📋 _Visible when Generate Collections is enabled_

</details>

<details>

<summary><strong>Capture Property Deltas</strong> <code>bool</code></summary>

When enabled, per-instance property differences from the class default object are serialized on actor collection entries. This allows reconstructing per-actor overrides during spawning.

Default: `false`

📋 _Visible when Generate Collections is enabled_

</details>

#### Inherited Settings

This exporter inherits from the Level Data Exporter base class.

→ See Level Data Exporter documentation for base configuration.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Helpers/PCGExDefaultLevelDataExporter.h)
