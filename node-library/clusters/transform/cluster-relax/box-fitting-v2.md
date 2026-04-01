---
description: >-
  Advanced cluster relaxation using configurable bounding boxes with multiple
  separation strategies.
icon: function
---

# Box Fitting v2

### Overview

This relaxation operation detects overlapping bounding boxes around cluster vertices and pushes them apart. Unlike the basic Box Fitting, this version allows you to specify extents from attributes or constants, choose different separation strategies, and optionally use oriented bounds. This provides more control over how vertices with physical extents are spaced apart.

> This is a [relax-operation.md](relax-operation.md "mention")

### How It Works

1. **Extents Determination**: Reads box extents from an attribute or uses a constant value for all vertices.
2. **Box Construction**: Builds axis-aligned bounding boxes centered on each vertex position, expanded by padding.
3. **Overlap Detection**: Tests all pairs of vertices for box intersection.
4. **Separation Calculation**: Determines the separation direction based on the selected mode.
5. **Force Application**: Pushes overlapping boxes apart along the calculated direction.
6. **Iteration**: Repeats until convergence or iteration limit.

**Usage Notes**

* **Extents Attribute**: Use `$Extents` or `$LocalBounds` to read per-point box sizes.
* **Separation Modes**: Choose based on your topology — Minimum Penetration is fastest, Edge Direction respects connectivity, Centroid is most natural for organic layouts.
* **Oriented Bounds**: Enable for rotated elements, but note this is more computationally expensive.

### Behavior

```
Separation Modes:

Minimum Penetration:     Edge Direction:         Centroid:
    [A]                      [A]                    [A]
     ↓ (smallest axis)        ↘ (along edge)         ↘ (center-to-center)
    [B]                      [B]                    [B]
```

### Settings

<details>

<summary><strong>Extents Input</strong> <code>EPCGExInputValueType</code></summary>

How box extents are determined for each vertex.

| Option        | Description                                 |
| ------------- | ------------------------------------------- |
| **Constant**  | Use the same extents value for all vertices |
| **Attribute** | Read extents from a point attribute         |

Default: `Attribute`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Extents (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read extents from. Expects a vector representing half-size in each axis.

Default: `$Extents`

📋 _Visible when Extents Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Extents</strong> <code>FVector</code></summary>

Constant extents value applied to all vertices. Represents half-size in each axis.

Default: `(50, 50, 50)`

📋 _Visible when Extents Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Separation Mode</strong> <code>EPCGExBoxFittingSeparation</code></summary>

How to determine the direction for separating overlapping boxes.

| Option                  | Description                                                            |
| ----------------------- | ---------------------------------------------------------------------- |
| **Minimum Penetration** | Separate along the axis with the smallest overlap (fastest resolution) |
| **Edge Direction**      | Prefer separation along connected edge directions (maintains topology) |
| **Centroid**            | Separate directly away from each other's centers (most natural)        |

Default: `Minimum Penetration`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Padding</strong> <code>double</code></summary>

Additional padding added to each box's extents. Creates extra spacing between elements.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Oriented Bounds</strong> <code>bool</code></summary>

Whether to consider point rotation when computing bounds. More accurate for rotated elements but more computationally expensive.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This operation inherits common relaxation settings from its base class, including iteration count, influence/strength, edge length handling, and repulsion constant.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExBoxFittingRelax2.h)
