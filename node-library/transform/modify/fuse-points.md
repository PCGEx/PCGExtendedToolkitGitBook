---
description: Fuse points based on distance.
icon: circle
---

# Fuse Points

### Overview

Fuse Points merges nearby points into single representatives based on a distance threshold. Points within the fuse radius are grouped together and either blended into a new point or reduced to the most central point of the group. This is useful for cleaning up duplicate or near-duplicate points.

### How It Works

1. **Spatial Query**: Builds spatial data structures to find nearby points efficiently.
2. **Group Detection**: Identifies groups of points within the specified fuse distance.
3. **Point Resolution**: For each group, either blends all points together or selects the most central one.
4. **Attribute Handling**: Blends or carries over attributes according to the configured rules.
5. **Output**: Produces a reduced point set with fused groups represented by single points.

**Usage Notes**

* **Preserve Order**: When enabled, output points maintain the relative order of input points, which may be important for sequential operations.
* **Union Metadata**: The Point/Point Settings can optionally write metadata about which points were fused together.

### Behavior

```
Input Points (tolerance = 10):

  ●A (0,0)    ●B (5,0)    ●C (50,0)    ●D (52,0)
       └──fused──┘              └──fused──┘

Mode: Blend
Output: ●AB (2.5,0)         ●CD (51,0)

Mode: Keep Most Central
Output: ●A or ●B            ●C or ●D
        (whichever is closest to group center)
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExFusedPointOutput</code></summary>

How to resolve groups of fused points into a single output point.

| Option                | Description                                                             |
| --------------------- | ----------------------------------------------------------------------- |
| **Blend**             | Blend all points within the radius into a new averaged point            |
| **Keep Most Central** | Keep the existing point that's closest to the center of the fused group |

Default: `Blend`

</details>

<details>

<summary><strong>Point/Point Settings</strong> <code>FPCGExPointPointIntersectionDetails</code></summary>

Settings controlling how points are detected as candidates for fusion.

| Property             | Type                              | Description                                   |
| -------------------- | --------------------------------- | --------------------------------------------- |
| **Fuse Details**     | `FPCGExFuseDetails`               | Distance and tolerance settings for fusion    |
| **Point Union Data** | `FPCGExPointUnionMetadataDetails` | Optional metadata to write about point unions |
| **Edge Union Data**  | `FPCGExEdgeUnionMetadataDetails`  | Optional metadata for edge union tracking     |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve Order</strong> <code>bool</code></summary>

When enabled, maintains the relative order of points from the input. Fused groups are represented at the position of their first encountered point in the sequence.

Default: `true`

</details>

<details>

<summary><strong>Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

Defines how fused point properties and attributes are merged together.

📋 _Visible when Mode = Blend_

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Settings for preserving attributes and tags through the fusion process.

📋 _Visible when Mode = Blend_

//→ See TODO FPCGExCarryOverDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                         |
| ------- | ---------- | --------------------------------------------------- |
| **Out** | Point Data | Reduced point set with nearby points fused together |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/PCGExFusePoints.h)
