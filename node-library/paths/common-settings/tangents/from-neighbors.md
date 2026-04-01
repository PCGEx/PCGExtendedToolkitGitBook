---
description: Calculates tangents by averaging incoming and outgoing directions.
icon: function
---

# From Neighbors

### Overview

This tangent factory computes tangents by blending the incoming direction (from previous point) with the outgoing direction (to next point), creating a bisector that smoothly transitions between path segments. Both arrive and leave tangents use the same averaged direction, producing symmetric curvature that balances the influence of both neighbors.

### How It Works

1. **Direction Calculation**: Computes direction from previous to current point (incoming)
2. **Direction Calculation**: Computes direction from current to next point (outgoing)
3. **Direction Averaging**: Linearly interpolates between the two directions at 50% (averages them)
4. **Tangent Assignment**: Sets both arrive and leave tangents to the averaged direction
5. **Scaling Application**: Applies arrive and leave scale factors to the tangent vectors
6. **Tangent Output**: Returns scaled arrive and leave tangents (both in the same direction)

**Usage Notes**

* **Bisector Behavior**: The averaged direction bisects the angle between incoming and outgoing path segments, creating smooth transitions even at sharp corners.
* **Symmetric Tangents**: Both arrive and leave tangents point in the same direction, ensuring symmetric curvature around each point.
* **Flow-Based**: Unlike Catmull-Rom (which only considers neighbor positions), this method considers the flow of the path through the current point.
* **Scale Independence**: The averaging is based on direction, not magnitude. Tangent length is controlled separately via scale factors.
* **Edge Points**: First and last points may receive special handling depending on parent node configuration.

### Behavior

```
Point sequence: P0 → P1 → P2 → P3

For P1:
  IncomingDir = P1 - P0 (direction from prev to current)
  OutgoingDir = P2 - P1 (direction from current to next)

  AveragedDir = Lerp(IncomingDir, OutgoingDir, 0.5)

  Arrive tangent: AveragedDir * ArriveScale
  Leave tangent: AveragedDir * LeaveScale

Both tangents point in the averaged direction.
```

**Tangent Direction (Bisector):**

```
Points arranged as:
  P0 (prev) → P1 (current) → P2 (next)

IncomingDir: P0 → P1
OutgoingDir: P1 → P2

AveragedDir bisects the angle between the two directions,
creating a tangent that smoothly transitions from incoming to outgoing.

       P2
      /
     / OutgoingDir
    /
  P1 ← AveragedDir (bisector)
    \
     \ IncomingDir
      \
       P0
```

**Averaging Effect:**

```
Sharp corner (90° turn):
  Incoming: →
  Outgoing: ↓
  Averaged: ↘ (diagonal bisector)

Gentle curve (shallow angle):
  Incoming: →
  Outgoing: ↗
  Averaged: → (nearly straight, slight upward bias)

Straight line:
  Incoming: →
  Outgoing: →
  Averaged: → (no change)
```

**Scaling Effect:**

```
ArriveScale = 1.0, LeaveScale = 1.0:
  Standard averaged tangents
  Balanced curvature

ArriveScale = 2.0, LeaveScale = 2.0:
  Longer tangents
  Wider, smoother curves

ArriveScale = 0.5, LeaveScale = 0.5:
  Shorter tangents
  Tighter curves

Different arrive/leave scales:
  Same direction, different magnitudes
  Creates asymmetric influence
```

**Comparison with Other Methods:**

```
Auto Tangents:
  Arrive: → Prev
  Leave: → Next
  (Opposite directions)

Catmull-Rom Tangents:
  Both: 0.5 * (Next - Prev)
  (Ignores current position)

From Neighbors Tangents:
  Both: 0.5 * (Incoming + Outgoing)
  (Considers path flow through current point)
```

**Symmetric Curvature:**

```
Because both arrive and leave tangents share the same direction:
- Smooth transitions around each point
- No abrupt direction changes
- Curvature balanced between incoming and outgoing segments
- Works well for paths with varying angles
```

Good for: smooth transitions, angle bisection, balanced curvature, flow-based tangents, direction averaging

### Settings

This factory has no configurable properties of its own. Tangent scaling is controlled by the parent node's **Scaling** settings (Arrive Scale and Leave Scale).

#### Inherited Settings

The scaling parameters are defined in the parent context where this factory is used:

* **Arrive Scale Input**: Constant or Attribute source for arrive tangent scale
* **Arrive Scale**: Scale factor for arrive tangents (default: 1.0)
* **Leave Scale Input**: Constant or Attribute source for leave tangent scale
* **Leave Scale**: Scale factor for leave tangents (default: 1.0)

→ See Write Tangents for tangent scaling configuration.

### Practical Examples

**Smooth Path Flow:**

```
[Path Points with varying angles]
  ↓
[Write Tangents] Tangents: From Neighbors, Scales: 1.0
  ↓
[Points with bisector tangents for smooth angular transitions]
```

**Corner Smoothing:**

```
[Path with sharp turns]
  ↓
[Write Tangents] Tangents: From Neighbors, Arrive Scale: 1.5, Leave Scale: 1.5
  ↓
[Points with longer bisector tangents rounding out corners]
```

**Tight Directional Flow:**

```
[Path Points]
  ↓
[Write Tangents] Tangents: From Neighbors, Arrive Scale: 0.4, Leave Scale: 0.4
  ↓
[Points with short tangents following direction closely]
```

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Tangents/PCGExTangentsFromNeighbors.h)
