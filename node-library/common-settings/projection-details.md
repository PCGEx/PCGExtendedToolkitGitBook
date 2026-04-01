---
description: >-
  Configures how 3D points are projected onto a 2D plane for geometric
  operations.
icon: sliders-simple
---

# Projection Details

### Overview

This settings block controls the projection of 3D point data onto a 2D plane, which is required for operations like Delaunay triangulation, Voronoi diagrams, Clipper2 boolean operations, and other 2D geometric algorithms. You can specify an explicit projection normal or let the system compute a best-fit plane automatically from the point distribution.

### How It Works

1. **Select Method**: Choose between explicit normal or automatic best-fit
2. **Define Plane**: Either provide a normal vector or let points determine the plane
3. **Project Points**: 3D coordinates are flattened onto the projection plane
4. **Process in 2D**: Geometric operations run on the projected coordinates
5. **Restore Depth**: Results are un-projected back to 3D space

### Behavior

```
3D Points                    Projection Plane              2D Result
    •                             |                          •
   •  •        Normal →           |    →                    • •
  •    •       (Up Vector)        |                        •   •
    •                             |                          •

Method: Normal
  Uses explicit vector to define projection plane

Method: Best Fit
  Computes plane that minimizes distance to all points
  (useful for non-axis-aligned point clouds)
```

### Settings

<details>

<summary><strong>Method</strong> <code>EPCGExProjectionMethod</code></summary>

Determines how the projection plane is established.

| Option       | Description                                                           |
| ------------ | --------------------------------------------------------------------- |
| **Normal**   | Uses an explicit normal vector to define the projection plane         |
| **Best Fit** | Automatically computes the best-fit plane from the point distribution |

Default: `Normal`

</details>

<details>

<summary><strong>Projection Vector</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

The normal vector defining the projection plane. Points are projected perpendicular to this direction. Can be a constant value or read from an attribute.

* **Constant**: Use a fixed vector (default: Up/+Z)
* **Attribute**: Read from a vector attribute on the data
* **Local**: When enabled, the vector is in local space rather than world space

Default: `Up Vector (0, 0, 1)`

📋 _Visible when Method = Normal_

</details>

**Usage Notes**

* **Best Fit**: Computes a plane that minimizes the average distance from all points, ideal for point clouds that lie roughly on a tilted or curved surface.
* **Data Attribute**: The default looks for a `@Data.Projection` attribute, allowing per-collection projection directions.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Math/PCGExProjectionDetails.h)
