---
description: >-
  Configures how overlapping points are detected and merged during fuse
  operations.
icon: sliders-simple
---

# Point ∩ Point

### Overview

This settings block controls point-to-point intersection detection and merging. When points from the same or different data sets are close enough to be considered overlapping, these settings determine the tolerance for detection, the method used for spatial grouping, and what metadata is written to track merged points. This is fundamental to fuse and clustering operations.

### How It Works

1. **Detect Overlaps**: Find points within the specified tolerance distance
2. **Group Points**: Use voxel grid or octree to efficiently cluster nearby points
3. **Merge to Union**: Combine overlapping points into single union points
4. **Write Metadata**: Optionally mark unions and record their source count

### Behavior

```
Point Fusing Example:

Before (tolerance = 5):
    A(0,0)  B(2,0)  C(10,0)  D(12,0)
    └───┬───┘       └───┬───┘
     Within           Within
    tolerance        tolerance

After:
    AB(1,0)          CD(11,0)
    (union of 2)     (union of 2)

Metadata (if enabled):
  AB: bIsUnion=true, UnionSize=2
  CD: bIsUnion=true, UnionSize=2
```

### Settings

<details>

<summary><strong>Fuse Details</strong> <code>FPCGExFuseDetails</code></summary>

Core fusing configuration including tolerance, distance calculation, and spatial grouping method.

| Sub-Setting                  | Description                                                 |
| ---------------------------- | ----------------------------------------------------------- |
| **Tolerance**                | Maximum distance for points to be considered overlapping    |
| **Component-Wise Tolerance** | Use separate X/Y/Z tolerances instead of spherical          |
| **Source/Target Distance**   | Which point reference to use (Center, Bounds Min/Max, etc.) |
| **Fuse Method**              | Spatial grouping algorithm (Voxel grid or Octree)           |
| **Voxel Grid Offset**        | Offset applied to voxel grid alignment                      |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Point Union Data</strong> <code>FPCGExPointUnionMetadataDetails</code></summary>

Controls metadata written to points that result from merging.

| Sub-Setting              | Description                                         |
| ------------------------ | --------------------------------------------------- |
| **Write Is Union**       | Write a boolean marking points created from unions  |
| **Is Union Attribute**   | Name of the boolean attribute (default: `bIsUnion`) |
| **Write Union Size**     | Write the count of points merged into each union    |
| **Union Size Attribute** | Name of the count attribute (default: `UnionSize`)  |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Union Data</strong> <code>FPCGExEdgeUnionMetadataDetails</code></summary>

Controls metadata written to edges when fusing cluster data. Similar to Point Union Data but for edge merging.

📋 _Visible when the operation supports edges (cluster fusing)_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Details/PCGExIntersectionDetails.h)
