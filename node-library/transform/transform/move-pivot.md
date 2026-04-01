---
description: Move pivot point relative to its bounds.
icon: circle
---

# Move Pivot

### Overview

Move Pivot repositions each point within its own bounding box using normalized UVW coordinates. This allows shifting the conceptual "anchor point" of each point from its center to any position within its bounds, such as a corner, edge center, or any arbitrary location. The point's position is adjusted while its bounds remain the same size.

### How It Works

1. **Bounds Calculation**: Computes the bounding box for each point based on the selected bounds reference.
2. **UVW Sampling**: Interprets UVW values as normalized coordinates within the bounds (0 = min, 0.5 = center, 1 = max).
3. **Position Offset**: Calculates the offset from current position to the UVW position within bounds.
4. **Transform Update**: Moves each point by the calculated offset.

**Usage Notes**

* **Coordinate System**: UVW maps to the point's local axes (U = local X, V = local Y, W = local Z).
* **Center Origin**: Default values of (0, 0, 0) represent the bounds minimum corner. Use (0.5, 0.5, 0.5) for center.

### Behavior

```
UVW = (0.5, 0.5, 0)  →  Pivot at bottom center
UVW = (0, 0, 0)      →  Pivot at minimum corner
UVW = (1, 1, 1)      →  Pivot at maximum corner
UVW = (0.5, 0.5, 0.5)→  Pivot at center (default PCG behavior)

Side view of point bounds:
┌─────────┐
│         │  UVW (0.5, 0.5, 1) = top center
│    ●────┼──→ Point moves so pivot is at UVW position
│         │  UVW (0.5, 0.5, 0) = bottom center
└─────────┘
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>UVW</strong> <code>FPCGExUVW</code></summary>

Normalized position within bounds where the pivot should be placed.

//→ See TODO FPCGExUVW

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                           |
| ------- | ---------- | ----------------------------------------------------- |
| **Out** | Point Data | Points with positions adjusted to new pivot locations |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/PCGExMovePivot.h)
