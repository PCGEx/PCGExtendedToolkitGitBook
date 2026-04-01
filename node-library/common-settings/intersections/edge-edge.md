---
description: >-
  Configures how crossing edges are detected and processed during cluster
  fusing.
icon: sliders-simple
---

# Edge ∩ Edge

### Overview

This settings block controls edge-to-edge intersection detection. When two edges cross each other (within tolerance), a new point is created at the intersection and the edges are split accordingly. Angle filtering allows rejecting near-parallel or near-perpendicular crossings, and metadata can mark the resulting intersection points.

### How It Works

1. **Find Crossings**: Detect where edges cross within tolerance distance
2. **Filter by Angle**: Optionally reject crossings below/above angle thresholds
3. **Create Points**: Generate new vertices at intersection locations
4. **Split Edges**: Original edges are subdivided at crossing points
5. **Write Metadata**: Optionally mark points created from crossings

### Behavior

```
Edge-Edge Intersection:

    A ●─────────────────● B
              ╲
               ╲
                ╲
    C ●──────────╳───────● D
                  ╲
                   ● E

Result:
    A ●────────●────────● B
               │
    C ●────────●────────● D
           (crossing point created,
            edges split at intersection)
```

### Settings

<details>

<summary><strong>Enable Self Intersection</strong> <code>bool</code></summary>

When enabled, edges from the same cluster can intersect each other. When disabled, only edges from different clusters are checked.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Maximum distance between edges to be considered as crossing. Accounts for edges that nearly touch but don't perfectly intersect.

Default: `DBL_INTERSECTION_TOLERANCE`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Min Angle</strong> <code>bool</code></summary>

Enables minimum angle filtering. When enabled, crossings where edges meet at angles below the threshold are rejected.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Angle</strong> <code>double</code></summary>

The minimum angle (in degrees) between edges for a valid crossing. Filters out near-parallel intersections that may be numerical artifacts.

Default: `0`

📋 _Visible when Use Min Angle = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Max Angle</strong> <code>bool</code></summary>

Enables maximum angle filtering. When enabled, crossings where edges meet at angles above the threshold are rejected.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angle</strong> <code>double</code></summary>

The maximum angle (in degrees) between edges for a valid crossing. Filters out near-perpendicular intersections if desired.

Default: `90`

📋 _Visible when Use Max Angle = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Crossing</strong> <code>bool</code></summary>

When enabled, writes a boolean attribute to mark points created from edge crossings.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Crossing Attribute Name</strong> <code>FName</code></summary>

The name of the boolean attribute marking crossing points.

Default: `bCrossing`

📋 _Visible when Write Crossing = true_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Details/PCGExIntersectionDetails.h)
