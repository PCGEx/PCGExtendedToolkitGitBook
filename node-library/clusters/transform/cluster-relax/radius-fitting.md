---
description: >-
  Relaxes cluster vertices by detecting and resolving overlaps between spheres
  centered on each vertex.
icon: function
---

# Radius Fitting

### Overview

This relaxation operation treats each vertex as a sphere with a configurable radius. When spheres overlap, vertices are pushed apart proportionally to the overlap amount. This is simpler and faster than box-based collision but works best for roughly spherical or uniform-sized elements.

> This is a [relax-operation.md](relax-operation.md "mention")

### How It Works

1. **Radius Assignment**: Each vertex gets a collision radius from an attribute or constant value.
2. **Overlap Detection**: Tests all pairs of vertices for sphere intersection (distance < sum of radii).
3. **Force Calculation**: When spheres overlap, calculates a repulsion force based on overlap depth and distance.
4. **Position Update**: Applies accumulated forces to push overlapping vertices apart.
5. **Iteration**: Repeats for the configured number of iterations until convergence.

**Usage Notes**

* **Radius Attribute**: Use `$Extents.Length` to derive radius from point bounds, or specify a custom attribute.
* **Uniform Elements**: Works best when elements are roughly spherical; for elongated shapes, consider Box Fitting.
* **Performance**: Faster than box-based methods due to simpler overlap calculations.

### Behavior

```
Before:                         After Relaxation:

   (A)====(B)                   (A)    (B)
   (spheres overlap)            (spheres separated)

Overlapping spheres push apart along their center-to-center vector.
```

### Settings

<details>

<summary><strong>Radius Input</strong> <code>EPCGExInputValueType</code></summary>

How the collision radius is determined for each vertex.

| Option        | Description                          |
| ------------- | ------------------------------------ |
| **Constant**  | Use the same radius for all vertices |
| **Attribute** | Read radius from a point attribute   |

Default: `Attribute`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Radius (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the collision radius from.

Default: `$Extents.Length`

📋 _Visible when Radius Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Constant collision radius applied to all vertices.

Default: `100`

📋 _Visible when Radius Input = Constant_

⚡ PCG Overridable

</details>

#### Inherited Settings

This operation inherits common fitting relaxation settings from its base class, including:

* Repulsion Constant
* Edge Fitting mode
* Spring Constant
* Time Step

See Fitting Relax Base for details.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExRadiusFittingRelax.h)
