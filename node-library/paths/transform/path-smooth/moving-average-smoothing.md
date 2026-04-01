---
description: >-
  Smoothing method that gathers neighbors within a sliding window of sequential
  points along the path.
icon: function
---

# Moving Average Smoothing

### Overview

This smoothing method collects neighboring points within a configurable window centered on each target point. Neighbors closer to the center of the window receive higher weights, creating a smooth falloff. The gathered neighbors and their weights are then passed to BlendOps for the actual property/attribute blending.

### How It Works

1. **Define Window**: For each point, create a window spanning `Smoothing` points in each direction along the path.
2. **Weight Neighbors**: Apply triangular weighting - neighbors closer to the center of the window receive higher influence.
3. **Handle Boundaries**: For open paths, use IndexSafety to control how the window behaves at path endpoints.
4. **Pass to BlendOps**: The gathered neighbors and computed weights are used by the parent node's BlendOps configuration.

**Usage Notes**

* **Path-Order Aware**: Only considers neighbors that are sequential in the path. Two points that are spatially close but far apart in path order won't influence each other.
* **Closed Loops**: For closed paths, the window automatically wraps around (uses Tile behavior internally).
* **Open Paths**: Use IndexSafety to control edge behavior - whether the window shrinks, wraps, clamps, or bounces at endpoints.

### Behavior

```
Moving Average with window size 2:

Window around point B:
        ┌───────────────┐
        │   A   B   C   │
        └───────────────┘
            ↓   ↓   ↓
  weight: 0.5  1.0  0.5

Neighbors A and C are gathered with weights
based on distance from center (B).

BlendOps then use these weights to blend
properties/attributes from A and C onto B.
```

### Settings

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle window indices that fall outside the path bounds (for open paths).

| Option     | Description                                               |
| ---------- | --------------------------------------------------------- |
| **Ignore** | Skip out-of-bounds indices (window shrinks at endpoints). |
| **Tile**   | Wrap around to the other end (treats path as circular).   |
| **Clamp**  | Clamp to the nearest valid index.                         |
| **Yoyo**   | Bounce back at boundaries.                                |

Default: `Ignore`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Smoothing/PCGExMovingAverageSmoothing.h)
