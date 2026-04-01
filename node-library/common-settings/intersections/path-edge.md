---
description: >-
  Configures how intersections between edges or path segments are detected and
  processed.
icon: sliders-simple
---

# Path ∩ Edge

### Overview

This settings block controls intersection detection between line segments, whether from cluster edges or path segments. It provides tolerance settings for numerical precision, angle constraints to filter which intersections are valid, and options to mark intersection points with attributes. This is used when cutting clusters with paths or finding where paths cross each other.

### How It Works

1. **Detect Intersections**: Find where segments cross within tolerance
2. **Filter by Angle**: Optionally reject intersections below/above angle thresholds
3. **Mark Results**: Optionally write an attribute to identify crossing points
4. **Output Points**: Valid intersections become new points in the output

### Behavior

```
Edge Intersection Detection:

    A ────────×──────── B
              │
    C ────────┼──────── D
              │
              × = Intersection point

Angle Filtering:
┌─────────────────────────────────────────┐
│ Min Angle: Rejects near-parallel edges  │
│ Max Angle: Rejects near-perpendicular   │
│                                         │
│     ∠ < MinAngle → Rejected             │
│     ∠ > MaxAngle → Rejected             │
└─────────────────────────────────────────┘
```

### Settings

<details>

<summary><strong>Enable Self Intersection</strong> <code>bool</code></summary>

When enabled, allows detection of intersections within the same path or cluster (self-crossing). When disabled, only intersections between different paths/clusters are detected.

Default: `true`

📋 _Visible when the operation supports self-intersection_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

The distance threshold for considering two segments as intersecting. Smaller values require more precise crossings; larger values catch near-misses.

Default: `DBL_INTERSECTION_TOLERANCE` (system default)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Min Angle</strong> <code>bool</code></summary>

Enables minimum angle filtering. When enabled, intersections where segments meet at angles below the threshold are rejected.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Angle</strong> <code>double</code></summary>

The minimum angle (in degrees) between segments for a valid intersection. Rejects near-parallel crossings that may be numerical artifacts.

Default: `0`

📋 _Visible when Use Min Angle = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Max Angle</strong> <code>bool</code></summary>

Enables maximum angle filtering. When enabled, intersections where segments meet at angles above the threshold are rejected.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angle</strong> <code>double</code></summary>

The maximum angle (in degrees) between segments for a valid intersection. Useful for filtering out perpendicular crossings.

Default: `90`

📋 _Visible when Use Max Angle = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Crossing</strong> <code>bool</code></summary>

When enabled, writes a boolean attribute to mark points that were created from intersections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Crossing Attribute Name</strong> <code>FName</code></summary>

The name of the boolean attribute written to intersection points.

Default: `bIsCrossing`

📋 _Visible when Write Crossing = true_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Paths/PCGExPathIntersectionDetails.h)
