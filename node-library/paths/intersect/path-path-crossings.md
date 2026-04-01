---
description: Find crossing points between & inside paths.
icon: circle
---

# Path × Path Crossings

### Overview

This node detects where path segments intersect with other path segments, either between different paths or within the same path (self-intersection). New points can be inserted at crossing locations with metadata about the intersection.

### How It Works

1. **Build Spatial Index**: Path segments are indexed for efficient intersection testing.
2. **Find Crossings**: Each segment is tested against potentially intersecting segments based on filtering rules.
3. **Insert Points**: New points are created at crossing locations along each affected path.
4. **Blend Attributes**: Crossing points receive blended attributes from the path's neighboring points and optionally from the crossing path.
5. **Tag Paths**: Paths are optionally tagged based on whether they have crossings.

**Usage Notes### Point Insertion Only**

This node inserts new points at crossing locations within the existing paths and optionally flags them. It does **not** merge paths or create branching structures. For merging paths at crossings or building connected networks, use Path to Cluster instead.

* **Self-Intersection**: When enabled, paths are tested against their own segments (useful for detecting where a path crosses over itself).
* **Cut/Cutter Tags**: Use tags to control which paths can be cut and which paths do the cutting. This allows asymmetric intersection behavior.
* **Angle Filtering**: Use min/max angle constraints to only detect crossings at certain angles (e.g., only perpendicular crossings).

### Behavior

```
Two paths crossing:

Path A:    ●───────●───────●
                    \
Path B:    ●─────────●─────────●
                      \
                       ●

After processing, new crossing point inserted on both paths:

Path A:    ●───────●───●───●
                       ↑
                    Crossing
Path B:    ●─────────●─●─────●
```

### Inputs

| Pin                    | Type             | Description                                |
| ---------------------- | ---------------- | ------------------------------------------ |
| **In**                 | Points           | Paths to test for crossings                |
| **Can Cut Filters**    | Filter Factories | Point filters for segments that can cut    |
| **Can Be Cut Filters** | Filter Factories | Point filters for segments that can be cut |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Self Intersection Only</strong> <code>bool</code></summary>

Only test each path against itself, ignoring other paths. Useful for detecting where a single path crosses over itself.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Can Be Cut Tag</strong> <code>FName</code></summary>

Tag filter for paths that can be cut. Only paths with this tag (or without, if inverted) will receive crossing points.

Default: None (all paths can be cut)

📋 _Visible when Self Intersection Only = false_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Can Cut Tag</strong> <code>FName</code></summary>

Tag filter for paths that can cut others. Only paths with this tag (or without, if inverted) will be used as cutters.

Default: None (all paths can cut)

📋 _Visible when Self Intersection Only = false_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Create Point At Crossings</strong> <code>bool</code></summary>

Insert new points at crossing locations. When disabled, only metadata and tags are applied without modifying point counts.

Default: `true`

</details>

<details>

<summary><strong>Intersection Details</strong> <code>FPCGExPathEdgeIntersectionDetails</code></summary>

Configuration for how edge intersections are detected.

//→ See TODO FPCGExPathEdgeIntersectionDetails

</details>

<details>

<summary><strong>Blending</strong> <code>UPCGExSubPointsBlendInstancedFactory</code></summary>

How crossing point attributes are blended from neighboring path points.

| Option            | Description                         |
| ----------------- | ----------------------------------- |
| **Inherit Start** | Inherit from segment's start point. |
| **Inherit End**   | Inherit from segment's end point.   |
| **None**          | No blending.                        |

⚡ PCG Overridable

</details>

***

#### Cross Blending

<details>

<summary><strong>Do Cross Blending</strong> <code>bool</code></summary>

Enable blending attributes from the crossing (other) path into the crossing point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Crossing Carry Over</strong> <code>FPCGExCarryOverDetails</code></summary>

Which attributes and tags to carry over from the crossing path.

📋 _Visible when Do Cross Blending = true_

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Crossing Blending</strong> <code>FPCGExBlendingDetails</code></summary>

How attributes from the crossing path are blended.

📋 _Visible when Do Cross Blending = true_

//→ See TODO FPCGExBlendingDetails

</details>

***

#### Output Attributes

<details>

<summary><strong>Write Alpha</strong> <code>bool</code></summary>

Write the crossing position as an alpha value (0-1) along the segment.

Default: `false` · Attribute: `Alpha` · Default Value: `-1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Orient Crossing</strong> <code>bool</code></summary>

Orient crossing points to face along the crossing direction.

Default: `false` · Axis: `Forward`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Cross Direction</strong> <code>bool</code></summary>

Write the direction of the crossing path at the intersection.

Default: `false` · Attribute: `Cross` · Default Value: `(0,0,0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Is Point Crossing</strong> <code>bool</code></summary>

Mark points that are crossings with a boolean attribute.

Default: `false` · Attribute: `IsPointCrossing`

⚡ PCG Overridable

</details>

***

#### Tagging

<details>

<summary><strong>Tag If Has Crossing</strong> <code>bool</code></summary>

Add a tag to paths that have at least one crossing.

Default: `false` · Tag: `HasCrossings`

</details>

<details>

<summary><strong>Tag If Has No Crossings</strong> <code>bool</code></summary>

Add a tag to paths that have no crossings.

Default: `false` · Tag: `HasNoCrossings`

</details>

<details>

<summary><strong>Omit Uncuttable From Output</strong> <code>bool</code></summary>

Exclude paths that are only cutters (can't be cut themselves) from the output.

Default: `false`

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathCrossings.h)
