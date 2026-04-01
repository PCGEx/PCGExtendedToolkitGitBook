---
description: Removes edges that overlap or pass too close to other edges in 3D space.
icon: function
---

# Refine : Overlap

### Overview

This edge refinement operation detects pairs of edges that pass within a specified tolerance of each other and removes one of them based on the Keep setting. This is useful for cleaning up clusters where edges cross or nearly intersect, reducing visual clutter and topological complexity. You can filter which overlaps are considered by specifying an angular range between the overlapping edges.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Spatial Query**: Uses an octree to efficiently find potentially overlapping edge pairs.
2. **Distance Check**: Calculates the minimum distance between each pair of edges.
3. **Angle Filter**: Optionally filters overlaps based on the angle between the two edges.
4. **Overlap Resolution**: When edges overlap within tolerance, removes either the shortest or longest based on settings.

**Usage Notes**

* **Tolerance**: Smaller values only detect near-intersections; larger values catch edges that pass nearby.
* **Angle Filtering**: Use the angle range to only consider overlaps between edges at certain relative angles (e.g., only nearly parallel or only crossing).
* **Keep Selection**: "Longest" keeps more significant connections; "Shortest" preserves local detail.

### Behavior

```
Before (edges nearly intersect):    After (Keep = Longest):

    A-----------B                   A-----------B
         X  (overlap)
    C-----------D

Edge A-B and C-D overlap → C-D removed (shorter)
```

### Settings

<details>

<summary><strong>Keep</strong> <code>EPCGExEdgeOverlapPick</code></summary>

Determines which edge to keep when two edges overlap.

| Option       | Description                                  |
| ------------ | -------------------------------------------- |
| **Shortest** | Keep the shorter edge, remove the longer one |
| **Longest**  | Keep the longer edge, remove the shorter one |

Default: `Longest`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Distance threshold at which two edges are considered overlapping. Edges passing within this distance of each other will trigger overlap resolution.

Default: `DBL_INTERSECTION_TOLERANCE` (very small)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Min Angle</strong> <code>bool</code></summary>

Enable minimum angle filtering for overlap detection.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Angle</strong> <code>double</code></summary>

Minimum angle (in degrees) between edges for an overlap to be considered. Overlaps between edges at angles below this are ignored.

Default: `0`

📋 _Visible when Use Min Angle is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Max Angle</strong> <code>bool</code></summary>

Enable maximum angle filtering for overlap detection.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angle</strong> <code>double</code></summary>

Maximum angle (in degrees) between edges for an overlap to be considered. Overlaps between edges at angles above this are ignored.

Default: `90`

📋 _Visible when Use Max Angle is enabled_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineRemoveOverlap.h)
