---
description: Orients points by averaging the directions to neighboring path points.
icon: function
---

# Orient Average

### Overview

This orientation method computes point orientation by blending the direction toward the next point with the direction from the previous point. This creates smooth, balanced orientations that follow the path's local curvature.

### How It Works

1. **Get Directions**: For each point, compute the direction vector to the next point and the direction from the previous point.
2. **Average**: Blend the two directions together (50/50 lerp) to get a smoothed forward direction.
3. **Build Transform**: Construct the orientation using the averaged direction as the forward axis and the configured up axis.

**Usage Notes**

* **Smooth Transitions**: Because this method averages neighboring directions, it produces smoother orientation transitions than methods that only look at one neighbor.
* **Path Endpoints**: At the start and end of open paths, only one neighbor direction is available, so the orientation may differ from interior points.
* **Symmetric**: The result is identical whether traversing the path forward or backward (aside from the direction multiplier).

### Behavior

```
Path points with orientation:

    Previous ●────────● Current ────────● Next
              \        ↗
               \      /
                \    /  Average of both
                 \  /   directions
                  \/

Forward = Lerp(DirToNext, -DirToPrev, 0.5)
```

### Settings

This factory has no additional settings beyond those inherited from the base orient factory.

#### Inherited Settings

→ See Orient Operation for: Orient Axis, Up Axis, and other base orientation settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Orient/PCGExOrientAverage.h)
