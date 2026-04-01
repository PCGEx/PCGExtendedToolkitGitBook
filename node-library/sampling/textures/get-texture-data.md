---
description: Create texture data objects from paths.
icon: circle
---

# Get Texture Data

### Overview

This node creates PCG Texture Data objects from texture or material paths stored in point attributes. It reads asset paths from points, resolves them to actual textures (extracting from materials if needed), and outputs ready-to-use texture data for sampling operations. This bridges the gap between asset references and PCG's texture sampling system.

### How It Works

1. **Read Paths**: Gets asset paths from the specified attribute.
2. **Resolve Assets**: Loads textures directly or extracts them from materials.
3. **Build Texture Data**: Creates PCG Texture Data objects with sampling parameters.
4. **Output References**: Optionally writes texture IDs back to points.

**Usage Notes**

* **Material Mode**: Extracts textures from materials using Texture Param definitions.
* **Texture Mode**: Loads textures directly from texture asset paths.
* **Filtering**: Supports Point (nearest) or Bilinear texture filtering.
* **Tiling**: Configure texture tiling and transform for sampling.

### Behavior

Texture Data Creation:

```
Points with $AssetPath attribute:
   P0: "/Game/Materials/M_Ground"
   P1: "/Game/Materials/M_Grass"
   P2: "/Game/Materials/M_Ground" (duplicate)

With Texture Param looking for "Diffuse":
   → Extracts diffuse texture from each material
   → Creates unique Texture Data per unique texture
   → Writes texture IDs to points

Output:
   Texture Data: [T_Ground_D, T_Grass_D]
   Points: P0($TextureId=0), P1($TextureId=1), P2($TextureId=0)
```

### Inputs

| Pin                | Type   | Description                                       |
| ------------------ | ------ | ------------------------------------------------- |
| **In**             | Points | Points with asset path attributes                 |
| **Texture Params** | Params | Texture parameter definitions (for material mode) |
| **Point Filters**  | Params | Optional filters for which points to process      |

### Settings

#### Source Configuration

<details>

<summary><strong>Source Type</strong> <code>EPCGExGetTexturePathType</code></summary>

Type of asset path stored in the attribute.

| Option            | Description                                                       |
| ----------------- | ----------------------------------------------------------------- |
| **Texture Path**  | Attribute contains direct texture asset paths                     |
| **Material Path** | Attribute contains material asset paths (requires Texture Params) |

Default: `Material Path`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Source Attribute Name</strong> <code>FName</code></summary>

Name of the attribute containing the asset path.

Default: `AssetPath`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Texture IDs</strong> <code>bool</code></summary>

When enabled, writes resolved texture identifiers to points as defined by Texture Params.

Default: `true`

📋 _Visible when Source Type = Material Path_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Build Texture Data</strong> <code>bool</code></summary>

When enabled, creates PCG Texture Data objects for each unique texture.

Default: `true`

📋 _Visible when Source Type = Material Path_

⚡ PCG Overridable

</details>

#### Texture Data Settings

<details>

<summary><strong>Filter</strong> <code>EPCGExTextureFilter</code></summary>

Method used to sample texel values.

| Option       | Description                                      |
| ------------ | ------------------------------------------------ |
| **Point**    | Takes the value of the texel the sample lands in |
| **Bilinear** | Interpolates values of four nearest texels       |

Default: `Bilinear`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform</strong> <code>FTransform</code></summary>

Transform applied to the texture surface.

Default: `Identity`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Absolute Transform</strong> <code>bool</code></summary>

When enabled, uses the transform as absolute rather than relative.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Color Channel</strong> <code>EPCGTextureColorChannel</code></summary>

Which color channel to use for single-value operations.

Default: `Alpha`

</details>

<details>

<summary><strong>Texel Size</strong> <code>float</code></summary>

Size of one texel in centimeters, used when converting to point data.

Default: `50.0`

</details>

#### Tiling

<details>

<summary><strong>Use Advanced Tiling</strong> <code>bool</code></summary>

Enables advanced tiling options for texture sampling.

Default: `false`

</details>

<details>

<summary><strong>Rotation</strong> <code>float</code></summary>

Rotation to apply when sampling texture (in degrees).

Default: `0`

📋 _Visible when Use Advanced Tiling is enabled_

</details>

<details>

<summary><strong>Tiling</strong> <code>FVector2D</code></summary>

Texture tiling factor (1.0 = no tiling).

Default: `(1.0, 1.0)`

📋 _Visible when Use Advanced Tiling is enabled_

</details>

<details>

<summary><strong>Center Offset</strong> <code>FVector2D</code></summary>

Offset applied to the tiling center.

Default: `(0, 0)`

📋 _Visible when Use Advanced Tiling is enabled_

</details>

<details>

<summary><strong>Use Tile Bounds</strong> <code>bool</code></summary>

When enabled, constrains sampling to a specific tile region.

Default: `false`

</details>

<details>

<summary><strong>Tile Bounds</strong> <code>FBox2D</code></summary>

Bounds of the tile region to sample from.

Default: `(-0.5, -0.5) to (0.5, 0.5)`

📋 _Visible when Use Advanced Tiling and Use Tile Bounds are enabled_

</details>

### Outputs

| Pin              | Type         | Description                           |
| ---------------- | ------------ | ------------------------------------- |
| **Out**          | Points       | Points with texture ID attributes     |
| **Texture Data** | Texture Data | PCG Texture Data objects for sampling |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExGetTextureData.h)
