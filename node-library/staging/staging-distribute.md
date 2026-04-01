---
description: Distribute PCGEx Asset Collection entries to points.
icon: circle
---

# Staging : Distribute

### Overview

This node assigns entries from an Asset Collection to individual points, preparing them for spawning or further processing. It handles entry selection, scale-to-fit calculations, justification within bounds, transform variations, and writes comprehensive staging data as point attributes or collection maps. The node supports multiple collection sources and extensive customization of how assets are distributed and fitted to points.

### How It Works

1. **Collection Loading**: Loads the specified collection from asset reference, attribute set, or per-point path attribute
2. **Entry Distribution**: Selects collection entries for each point using configured distribution mode (weighted random, sequential, etc.)
3. **Fitting Calculations**: Computes scale and translation to fit assets within point bounds according to scale-to-fit and justification settings
4. **Variation Application**: Applies randomized transform offsets, rotations, and scale adjustments
5.  **Data Output**: Writes staging data as point attributes or stores collection map references for later use

    <div data-gb-custom-block data-tag="hint" data-style="success" class="hint hint-success"><p>### No assets are loaded at runtime</p></div>

This node doesn't load or spawn any meshes, materials, or other assets during PCG execution. It relies entirely on **cached staging data** that the Asset Collection computes at editor-time. This includes bounds, material slots, socket transforms, and all other metadata needed for fitting and distribution.

This means staging is extremely fast — you're just reading pre-computed values and writing attributes, not touching the asset registry or loading packages.

{% hint style="info" %}
\### Collection Map: Chain distributions and merge freely

When using **Collection Map** output mode, the node generates a unique identifier for each referenced collection. This enables powerful workflows:

* **Chain multiple distribution passes** — Run staging multiple times with different collections or settings, and the maps stay distinct
* **Merge collection maps safely** — Combine points from different staging operations without conflicts
* **Defer spawning to the end** — Process, filter, and transform staged points throughout your graph, then spawn everything in one final step

This also enables **nested staging**: you can stage assets, then use their socket outputs or bounds to stage additional assets that fit within them — for example, placing props that respect the bounds of previously staged furniture.
{% endhint %}

**Usage Notes**

* **Collection Map Mode**: Most efficient for use with PCGEx spawning nodes - stores references without expanding all data
* **Attributes Mode**: Writes full asset data to points for use with standard PCG spawning or custom logic
* **Bounds Dependency**: Scale-to-fit and justification require valid bounds data in the collection staging
* **Material Picks**: For mesh collections, can output per-slot material variant selections

### Inputs

| Pin    | Type       | Description                                |
| ------ | ---------- | ------------------------------------------ |
| **In** | Point Data | Points to receive staged asset assignments |

### Outputs

| Pin     | Type       | Description                      |
| ------- | ---------- | -------------------------------- |
| **Out** | Point Data | Points with staging data written |

### Settings

#### Collection Source

<details>

<summary><strong>Collection Source</strong> <code>EPCGExCollectionSource</code></summary>

Determines where the asset collection reference comes from.

| Option             | Description                                            |
| ------------------ | ------------------------------------------------------ |
| **Asset**          | Use a single collection asset for all points           |
| **Attribute Set**  | Build a virtual collection from an attribute set input |
| **Path Attribute** | Read collection path from a per-point attribute        |

Default: `Asset`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Asset Collection</strong> <code>TSoftObjectPtr</code></summary>

The Asset Collection to distribute entries from when Collection Source is set to Asset.

Default: None

⚡ PCG Overridable

📋 _Visible when Collection Source = Asset_

</details>

<details>

<summary><strong>Attribute Set Details</strong> <code>FPCGExRoamingAssetCollectionDetails</code></summary>

Configuration for building a virtual collection from attribute set data when Collection Source is set to Attribute Set. Specifies which attributes contain asset paths, weights, and categories.

Default: AssetPath attribute

⚡ PCG Overridable

📋 _Visible when Collection Source = Attribute Set_

</details>

<details>

<summary><strong>Collection Path Attribute Name</strong> <code>FName</code></summary>

Name of the attribute containing the collection path when Collection Source is set to Path Attribute.

Default: `CollectionPath`

⚡ PCG Overridable

📋 _Visible when Collection Source = Path Attribute_

</details>

#### Output Configuration

<details>

<summary><strong>Output Mode</strong> <code>EPCGExStagingOutputMode</code></summary>

Controls how staging data is written to points.

| Option               | Description                                                                           |
| -------------------- | ------------------------------------------------------------------------------------- |
| **Point Attributes** | Write full asset data as point attributes (compatible with standard PCG spawning)     |
| **Collection Map**   | Store collection reference and pick index for efficient use with PCGEx spawning nodes |

Default: `Collection Map`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Asset Path Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to write the asset path to when Output Mode is Point Attributes.

Default: `AssetPath`

⚡ PCG Overridable

📋 _Visible when Output Mode = Point Attributes_

</details>

#### Distribution

<details>

<summary><strong>Distribution</strong> <code>FPCGExAssetDistributionDetails</code></summary>

Controls how collection entries are selected and assigned to points. Includes options for:

* **Category Filtering**: Optionally restrict selection to specific categories
* **Distribution Mode**: Selection strategy (weighted random, sequential, attribute-driven, etc.)
* **Seed Configuration**: Randomization seed components
* **Index Settings**: Configuration for index-based distribution modes

Default: Weighted Random distribution

⚡ PCG Overridable

</details>

<details>

<summary><strong>Distribution (Entry)</strong> <code>FPCGExMicroCacheDistributionDetails</code></summary>

Distribution settings for entry-level picks. For mesh collections, this controls how material variants are selected.

Default: Default distribution

⚡ PCG Overridable

</details>

#### Fitting

<details>

<summary><strong>Scale To Fit</strong> <code>FPCGExScaleToFitDetails</code></summary>

Controls how assets are scaled to fit within point bounds. Supports:

* **Fit Mode**: Uniform or per-axis scaling
* **Fit Strategy**: Scale to min, max, or specific axis extents
* Per-axis fit configuration (X, Y, Z)

Default: Uniform fit to minimum bounds

⚡ PCG Overridable

</details>

<details>

<summary><strong>Justification</strong> <code>FPCGExJustificationDetails</code></summary>

Controls how assets are positioned (justified) within point bounds after scaling. Each axis can be independently justified (center, min, max, or custom blend).

//→ See TODO FPCGExJustificationDetails

</details>

<details>

<summary><strong>Variations</strong> <code>FPCGExFittingVariationsDetails</code></summary>

Enables randomized transform variations applied after fitting calculations. Controls which transform components (offset, rotation, scale) receive variation.

Default: All disabled

</details>

#### Output Filtering

<details>

<summary><strong>Prune Empty Points</strong> <code>bool</code></summary>

Remove points that failed to receive a staging assignment (no valid entry selected).

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Do Filter Entry Type</strong> <code>bool</code></summary>

Enable filtering output based on collection entry type (Mesh, Actor, PCG Data Asset).

Default: `false`

</details>

<details>

<summary><strong>Entry Type Filter</strong> <code>FPCGExStagedTypeFilterDetails</code></summary>

Specifies which entry types to include or exclude from staging. Useful when using per-point collections with mixed asset types.

Default: All types allowed

⚡ PCG Overridable

📋 _Visible when Do Filter Entry Type is enabled_

</details>

#### Additional Outputs

<details>

<summary><strong>Write Entry Type</strong> <code>bool</code></summary>

Write the collection entry type (Mesh, Actor, etc.) to an attribute.

**Entry Type Attribute Name** `FName`: Name of the FName attribute to write type to.

Default: Disabled, attribute name `EntryType`

⚡ PCG Overridable (attribute name)

</details>

<details>

<summary><strong>Weight To Attribute</strong> <code>EPCGExWeightOutputMode</code></summary>

Write the entry's weight value to an attribute with optional normalization.

| Option                             | Description                                 |
| ---------------------------------- | ------------------------------------------- |
| **No Output**                      | Don't write weight                          |
| **Raw**                            | Write raw weight value                      |
| **Normalized**                     | Normalize weight to 0-1 range               |
| **Normalized Inverted**            | Inverted normalized weight (1 - normalized) |
| **Normalized to Density**          | Write normalized weight to point density    |
| **Normalized Inverted to Density** | Write inverted normalized weight to density |

**Weight Attribute Name** `FName`: Name of the attribute to write weight to.

Default: No Output, attribute name `AssetWeight`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Material Picks</strong> <code>bool</code></summary>

For mesh collections with material variants, output the selected material paths for each slot.

**Fixed Max Index** `int32`: Create dummy attributes up to this index for fixed-length material lists (0 = only valid indices).

**Prefix** `FName`: Attribute name prefix for material slot picks.

Default: Disabled, max index 0, prefix `Mat`

⚡ PCG Overridable

📋 _Visible when Output Mode ≠ Collection Map_

</details>

<details>

<summary><strong>Do Output Sockets</strong> <code>bool</code></summary>

Output socket transforms from staged assets as a separate point collection.

**Output Sockets** `FPCGExSocketOutputDetails`: Socket filtering and attribute configuration.

Default: Disabled

⚡ PCG Overridable (toggle)

</details>

<details>

<summary><strong>Write Translation</strong> <code>bool</code></summary>

Write the fitting translation offset applied during justification to an attribute. Useful for offsetting spline meshes.

**Translation Attribute Name** `FName`: Name of the FVector attribute to write offset to.

Default: Disabled, attribute name `FittingOffset`

⚡ PCG Overridable (attribute name)

</details>

#### Warnings and Errors

<details>

<summary><strong>Quiet Empty Collection Error</strong> <code>bool</code></summary>

Suppress warnings when the asset collection is empty or has no valid entries.

Default: `false`

</details>

#### Inherited Settings

This node inherits point filtering capabilities from its base class.

→ Point filters can be applied to control which points receive staging assignments

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExAssetStaging.h)
