---
description: Sample texture data using UV coordinates.
icon: circle
---

# Sample : Texture

### Overview

This node samples texture data at each point using UV coordinates from an attribute. It reads textures through the Texture Data and Texture Params inputs, allowing flexible sampling of multiple texture channels with custom attribute mapping. The sampling results are written to point attributes defined by the texture parameter configurations.

### How It Works

1. **Read UVs**: Gets UV coordinates from each point's attribute.
2. **Resolve Texture**: Matches points to textures via the texture lookup.
3. **Sample Textures**: Samples all configured texture parameters at the UV location.
4. **Write Results**: Outputs sampled values to specified attributes.

**Usage Notes**

* **UV Source**: Requires UV coordinates as a Vector2D attribute.
* **Texture Params**: Define which channels to sample and output attributes.
* **Texture Data**: Provides the actual texture assets to sample from.
* **Failed Samples**: Points that fail sampling can be pruned or tagged.

### Behavior

**Texture Sampling:**

```
Point P with UV = (0.3, 0.7):

Texture Params define:
   "BaseColor" → samples RGB to $BaseColor
   "Roughness" → samples R channel to $Roughness
   "Normal" → samples RGB to $Normal

Sampling:
   → Looks up texture via TextureID attribute
   → Samples at UV (0.3, 0.7)
   → Writes scaled values to output attributes
```

### Inputs

| Pin                | Type         | Description                                |
| ------------------ | ------------ | ------------------------------------------ |
| **In**             | Points       | Source points with UV coordinates          |
| **Texture Data**   | Texture Data | PCG Texture Data assets to sample from     |
| **Texture Params** | Params       | Texture parameter definitions for sampling |
| **Point Filters**  | Params       | Optional filters for source points         |

### Settings

#### UV Configuration

<details>

<summary><strong>UV Source</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute containing UV coordinates for texture sampling.

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Successes</strong> <code>bool</code></summary>

Adds a tag to collections where any sampling succeeded.

Default: `false`

</details>

<details>

<summary><strong>Tag If Has No Successes</strong> <code>bool</code></summary>

Adds a tag to collections where all sampling failed.

Default: `false`

</details>

#### Advanced

<details>

<summary><strong>Process Filtered Out As Fails</strong> <code>bool</code></summary>

When enabled, filtered-out points are marked as failed samples. Disable to preserve existing attribute values on filtered points.

Default: `true`

</details>

<details>

<summary><strong>Prune Failed Samples</strong> <code>bool</code></summary>

When enabled, removes points that failed to sample any texture.

Default: `false`

</details>

<details>

<summary><strong>Quiet Duplicate Sample Names Warning</strong> <code>bool</code></summary>

Suppresses warnings about duplicate sample attribute names across texture params.

Default: `false`

</details>

### Outputs

| Pin     | Type   | Description                      |
| ------- | ------ | -------------------------------- |
| **Out** | Points | Points with sampled texture data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleTexture.h)
