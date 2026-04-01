---
description: >-
  Orients points by distance-weighted blend of neighboring path point
  directions.
icon: function
---

# Orient Weighted

### Overview

This orientation method blends the directions to neighboring points using distance-based weighting. Points closer to one neighbor will have their orientation biased toward that neighbor's direction, creating more natural transitions along paths with uneven point spacing.

### How It Works

1. **Measure Distances**: Calculate the squared distance to both the previous and next points.
2. **Compute Weight**: Derive a blend weight from the ratio of distances, favoring the direction of the closer neighbor.
3. **Blend Directions**: Lerp between the direction to the previous point and the direction to the next point using the computed weight.
4. **Build Transform**: Construct the orientation using the weighted direction as the forward axis.

**Usage Notes**

* **Uneven Spacing**: This method is particularly useful for paths with non-uniform point spacing, where a simple 50/50 average would produce awkward orientations.
* **Weight Calculation**: The weight formula `(AB + BC) / min(AB, BC)` produces higher values when distances are more similar, and lower values when one neighbor is much closer.
* **Inverse Mode**: When inverted, the orientation favors the direction of the farther neighbor instead of the closer one.

### Behavior

```
Distance-based weighting:

Short segment    Long segment
  ●──────●───────────────────●
  Prev   Current              Next

  Weight biases toward Prev (closer neighbor)
  → Orientation aligned more with direction to Prev

With Inverse Weight:
  → Orientation aligned more with direction to Next (farther)
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Inverse Weight</strong> <code>bool</code></summary>

When enabled, inverts the distance weighting so the orientation favors the direction of the farther neighbor instead of the closer one.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Orient Operation for: Orient Axis, Up Axis, and other base orientation settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Orient/PCGExOrientWeighted.h)
