---
description: >-
  Relaxes cluster vertices by detecting and resolving overlaps between their
  axis-aligned bounding boxes.
icon: function
---

# Box Fitting

### Overview

This relaxation operation uses the local bounds of each vertex's point data to detect overlapping bounding boxes. When boxes overlap, vertices are pushed apart along their connecting vector proportionally to the overlap size. This is useful for spacing out elements that have physical extents, such as meshes or actors, to prevent visual intersection.

> This is a [relax-operation.md](relax-operation.md "mention")

### How It Works

1. **Box Calculation**: For each vertex, calculates an axis-aligned bounding box from its point's local bounds, expanded by the padding value.
2. **Overlap Detection**: Tests all pairs of vertices for box intersection.
3. **Force Calculation**: When boxes overlap, calculates a repulsion force based on the overlap size and direction.
4. **Position Update**: Applies accumulated forces to push overlapping vertices apart.
5. **Iteration**: Repeats for the configured number of iterations until convergence.

**Usage Notes**

* **Point Bounds**: This operation uses the `$LocalBounds` property of each point. Ensure your points have meaningful bounds set.
* **Padding**: Use padding to add extra spacing between boxes beyond their natural extents.
* **Axis-Aligned**: Boxes remain axis-aligned regardless of point rotation — for rotated bounds, consider other relaxation methods.

### Behavior

```
Before:                         After Relaxation:

   [A]=====[B]                  [A]     [B]
   (boxes overlap)              (boxes separated)

Overlapping boxes are pushed apart along their center-to-center vector.
```

### Settings

<details>

<summary><strong>Padding</strong> <code>double</code></summary>

A padding value added to each bounding box to increase spacing between vertices. Higher values result in more separation between elements.

Default: `10`

⚡ PCG Overridable

</details>

#### Inherited Settings

This operation inherits common relaxation settings from its base class, including:

* Iteration count
* Influence/strength
* Edge length handling
* Repulsion constant

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExBoxFittingRelax.h)
