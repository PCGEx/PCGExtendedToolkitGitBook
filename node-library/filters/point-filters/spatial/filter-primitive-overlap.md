---
description: >-
  Creates a filter definition that tests point OBB overlap against primitive
  component collision.
icon: circle-dashed
---

# Filter : Primitive Overlap

### Overview

The Primitive Overlap filter checks whether each point's oriented bounding box (OBB) overlaps with any of the provided primitive components' collision shapes. Points that overlap at least one primitive pass the filter. An octree is built from the primitive bounds for efficient broad-phase culling before performing precise overlap tests.

### How It Works

1. **Primitive Collection**: On preparation, all input primitive components are cached along with their world-space bounds, and an octree is built for fast spatial lookup.
2. **Broad Phase**: For each point, its OBB (derived from the selected bounds source and the point's transform) is tested against the octree to find nearby primitives.
3. **Narrow Phase**: For each candidate primitive, a precise `OverlapComponent` test is performed using the point's OBB as a collision shape, respecting position, rotation, and scale.
4. **Result**: The point passes if any primitive overlap is detected. If `Invert` is enabled, the logic flips — points pass only when they overlap _no_ primitives.

**Usage Notes**

* **Bounds source matters**: The bounds source controls the shape of the collision test on the point side. `ScaledBounds` uses the point's full scaled extents; other options may produce tighter or looser shapes.
* **Collision required on primitives**: The input primitive components must have collision data — the test uses Unreal's `OverlapComponent`, which relies on the component's collision geometry.
* **Octree acceleration**: The broad-phase octree means performance scales well even with many primitives, but very large or highly overlapping primitive bounds reduce the culling benefit.

### Inputs

| Pin            | Type           | Description                                 |
| -------------- | -------------- | ------------------------------------------- |
| **Primitives** | Primitive Data | Primitive components to test points against |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use on input points for building the overlap collision shape.

Default: `ScaledBounds`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

If enabled, inverts the result of the test — points that do _not_ overlap any primitive will pass instead.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Filter Provider Settings for shared filter options including missing data policy.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Filter** | Filter Factory | Filter definition for use with filter-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExPrimitiveFilter.h)
