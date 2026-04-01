---
description: Configures how points intersecting with edges are detected and processed.
icon: sliders-simple
---

# Point ∩ Edge

### Overview

This settings block controls point-to-edge intersection detection. When a point lies close enough to an edge (within tolerance), it can be snapped onto the edge and optionally marked with metadata. This is used during cluster fusing to detect where vertices from one cluster touch edges of another, enabling proper graph connectivity.

### How It Works

1. **Find Nearby Points**: Identify points within tolerance distance of edges
2. **Check Self-Intersection**: Optionally include edges connected to the point itself
3. **Snap Position**: Optionally move the point exactly onto the edge
4. **Write Metadata**: Optionally mark points that intersected edges

### Behavior

```
Point-Edge Intersection:

    A ●─────────────────● B     (Edge)
              ↑
              ● P              (Point within tolerance)
              │
         tolerance

Result (with snap):
    A ●────────●────────● B
               P (snapped onto edge)

Metadata (if enabled):
  P: bIsIntersector = true
```

### Settings

<details>

<summary><strong>Enable Self Intersection</strong> <code>bool</code></summary>

When enabled, points can intersect with edges they are directly connected to. When disabled, points only check edges from other clusters or paths.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fuse Details</strong> <code>FPCGExSourceFuseDetails</code></summary>

Controls the tolerance and distance calculation for intersection detection.

| Sub-Setting                  | Description                                 |
| ---------------------------- | ------------------------------------------- |
| **Tolerance**                | Maximum distance from edge for intersection |
| **Component-Wise Tolerance** | Use separate X/Y/Z tolerances               |
| **Source Distance**          | Point reference for distance calculation    |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Snap On Edge</strong> <code>bool</code></summary>

When enabled, intersecting points are moved to lie exactly on the edge at the closest position. This ensures precise alignment with the edge geometry.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Is Intersector</strong> <code>bool</code></summary>

When enabled, writes a boolean attribute to mark points that resulted from edge intersections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Is Intersector Attribute Name</strong> <code>FName</code></summary>

The name of the boolean attribute marking intersection points.

Default: `bIsIntersector`

📋 _Visible when Write Is Intersector = true_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Details/PCGExIntersectionDetails.h)
