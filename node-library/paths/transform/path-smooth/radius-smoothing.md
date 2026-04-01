---
description: >-
  Smoothing method that gathers all points within a spatial radius and weights
  them by distance.
icon: function
---

# Radius Smoothing

### Overview

This smoothing method finds all points within a configurable radius in 3D space, regardless of their position in the path sequence. Closer points receive higher weights using inverse squared distance falloff. The gathered neighbors and their weights are then passed to BlendOps for the actual property/attribute blending.

### How It Works

1. **Spatial Search**: Use an octree to efficiently find all points within the smoothing radius.
2. **Distance Weighting**: Weight each neighbor by inverse squared distance (closer points have more influence).
3. **Pass to BlendOps**: The gathered neighbors and computed weights are used by the parent node's BlendOps configuration.

**Usage Notes**

* **Ignores Path Order**: This method only considers 3D distance. Two points that are far apart in path order but spatially close will influence each other.
* **Self-Intersecting Paths**: Effective for paths that pass near themselves, where different sections should blend together.
* **Performance**: Uses octree spatial indexing for efficient neighbor lookup even with many points.

### Behavior

```
Radius smoothing (radius = R):

         ●───●───●
                  \
                   ●←── target point
                  /
         ●───●───●

All points within radius R of target
are gathered, regardless of path order.

Closer points = higher weight
(inverse squared distance falloff)

BlendOps then use these weights to blend
properties/attributes onto the target.
```

### Settings

This factory has no additional settings. The smoothing radius is controlled by the parent node's **Smoothing** parameter.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Smoothing/PCGExRadiusSmoothing.h)
