---
description: Apply fitting, justification and variations to staged points.
icon: circle
---

# Staging : Fitting

### Overview

This node applies scale-to-fit, justification, and optional transform variations to points that have already been staged. It reads bounds data either from a collection map (written by a prior staging node) or from a static mesh path attribute, then adjusts each point's transform so assets fit correctly within their bounds.

### How It Works

1. **Bounds Resolution**: Reads per-point bounds from either the collection map or by loading static meshes referenced by a path attribute
2. **Scale-to-Fit**: Scales each point's transform so the associated asset fits within the point's bounds according to the configured fit strategy
3. **Justification**: Offsets the asset within the remaining space after scaling, controlling alignment along each axis
4. **Variations** _(Collection Map mode only)_: Applies randomized offset, rotation, or scale adjustments before or after fitting
5. **Output**: Writes the adjusted transforms and optionally records the fitting translation offset as an attribute

**Usage Notes**

* **Post-Staging Node**: This node expects points that have already been staged — it applies fitting as a separate pass rather than during distribution.
* **Collection Map vs Mesh Attribute**: Collection Map mode uses pre-cached bounds from staging data, which is faster. Mesh Attribute mode loads actual static meshes to compute bounds, which is more flexible but heavier.
* **Variations Require Collection Map**: Per-asset variation limits are stored in collection entries, so variations are only available in Collection Map mode.
* **Point Filters**: Supports point filters to control which points receive fitting adjustments.

### Inputs

| Pin    | Type       | Description                       |
| ------ | ---------- | --------------------------------- |
| **In** | Point Data | Staged points to apply fitting to |

### Settings

#### Bounds Source

<details>

<summary><strong>Source</strong> <code>EPCGExFittingSource</code></summary>

Where to read asset bounds from.

| Option             | Description                                                                             |
| ------------------ | --------------------------------------------------------------------------------------- |
| **Collection Map** | Use staged entry data from a collection map to resolve bounds                           |
| **Mesh Attribute** | Read a static mesh path from an attribute and compute bounds from the mesh bounding box |

Default: `Collection Map`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Asset Path Attribute Name</strong> <code>FName</code></summary>

The name of the attribute containing the static mesh path to load and compute bounds from.

Default: `AssetPath`

⚡ PCG Overridable

📋 _Visible when Source = Mesh Attribute_

</details>

#### Fitting

<details>

<summary><strong>Scale To Fit</strong> <code>FPCGExScaleToFitDetails</code></summary>

Controls how assets are scaled to fit within point bounds. Supports uniform and per-axis scaling with configurable fit strategies.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Justification</strong> <code>FPCGExJustificationDetails</code></summary>

Controls how assets are positioned within point bounds after scaling. Each axis can be independently justified (center, min, max, or custom blend).

⚡ PCG Overridable

</details>

<details>

<summary><strong>Variations</strong> <code>FPCGExFittingVariationsDetails</code></summary>

Applies randomized transform variations to fitted points. Each transform component can be independently enabled and set to apply before or after fitting calculations.

| Property     | Type                  | Options                                   | Default    |
| ------------ | --------------------- | ----------------------------------------- | ---------- |
| **Offset**   | `EPCGExVariationMode` | Disabled · Before fitting · After fitting | `Disabled` |
| **Rotation** | `EPCGExVariationMode` | Disabled · Before fitting · After fitting | `Disabled` |
| **Scale**    | `EPCGExVariationMode` | Disabled · Before fitting · After fitting | `Disabled` |

Default: All `Disabled`

📋 _Visible when Source = Collection Map_

</details>

#### Output

<details>

<summary><strong>Prune Empty Points</strong> <code>bool</code></summary>

Remove points that have no valid staging entry (empty entry). Keeps output clean by discarding points that would spawn nothing.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Translation</strong> <code>bool</code></summary>

Write the fitting translation offset to an attribute. This is the translation added to the point transform by fitting and justification — useful for offsetting spline meshes.

Default: `false`

</details>

<details>

<summary><strong>Translation Attribute Name</strong> <code>FName</code></summary>

Name of the FVector attribute to write the fitting offset to.

Default: `FittingOffset`

⚡ PCG Overridable

📋 _Visible when Write Translation is enabled_

</details>

#### Inherited Settings

This node inherits point processing and filtering capabilities from its base class.

→ Point filters can be applied to control which points receive fitting adjustments

### Outputs

| Pin     | Type       | Description                            |
| ------- | ---------- | -------------------------------------- |
| **Out** | Point Data | Points with fitting transforms applied |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingFitting.h)
