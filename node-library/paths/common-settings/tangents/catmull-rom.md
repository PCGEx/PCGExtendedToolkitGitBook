---
description: Calculates tangents using Catmull-Rom spline interpolation.
icon: function
---

# Catmull-Rom

### Overview

This tangent factory implements Catmull-Rom spline tangent calculation, a classic interpolation method that creates smooth curves passing through all control points. Both arrive and leave tangents are computed as half the vector from the previous point to the next point, ensuring C1 continuity (smooth first derivatives) and producing naturally flowing curves without overshooting.

### How It Works

1. **Neighbor Retrieval**: For each point, retrieves the previous and next point positions
2. **Direction Calculation**: Computes the vector from previous point to next point (ignoring current position)
3. **Half-Vector Tangent**: Sets both arrive and leave tangents to half this direction vector
4. **Scaling Application**: Applies arrive and leave scale factors to the tangent vectors
5. **Tangent Output**: Returns scaled arrive and leave tangents (both in the same direction)

**Usage Notes**

* **Symmetric Tangents**: Unlike some tangent methods, Catmull-Rom produces parallel arrive and leave tangents, creating symmetric curvature around each point.
* **C1 Continuity**: The method guarantees smooth first derivatives across the curve, meaning no sudden direction changes at control points.
* **Proportional Length**: Tangent length automatically scales with neighbor distance, creating consistent curvature regardless of point spacing.
* **Edge Points**: First and last points may receive special handling depending on parent node configuration, as they lack one neighbor.
* **Classic Method**: This is a well-established spline interpolation technique widely used in computer graphics and animation.

### Behavior

```
Point sequence: P0 → P1 → P2 → P3

For P1:
  Direction = P2 - P0 (vector from prev to next)

  Arrive tangent: Direction * 0.5 * ArriveScale
  Leave tangent: Direction * 0.5 * LeaveScale

Both tangents point in the same direction.
```

**Tangent Direction (Symmetric):**

```
Points arranged as:
  P0 (prev) ← P1 (current) → P2 (next)

Direction vector: P0 → P2

Arrive: (P2 - P0) * 0.5
Leave:  (P2 - P0) * 0.5

Both tangents parallel, creating symmetric curvature.
```

**Catmull-Rom Properties:**

```
Characteristics:
- Passes through all control points
- C1 continuous (smooth first derivatives)
- Local control (changing a point affects only nearby segments)
- No overshooting (curve stays within control point convex hull)
- Tangent length proportional to neighbor distance

Tangent = 0.5 * (Next - Prev):
  The 0.5 factor ensures proper interpolation
  Creates balanced, natural-looking curves
```

**Scaling Effect:**

```
ArriveScale = 1.0, LeaveScale = 1.0:
  Standard Catmull-Rom interpolation
  Smooth, natural curves

ArriveScale = 2.0, LeaveScale = 2.0:
  Longer tangents
  Wider, gentler curves

ArriveScale = 0.5, LeaveScale = 0.5:
  Shorter tangents
  Tighter, more direct curves

Different arrive/leave scales:
  Asymmetric influence creates tension variation
```

**Comparison with Auto:**

```
Auto Tangents:
  Arrive: Points toward previous
  Leave: Points toward next
  → Opposite directions, follows path geometry

Catmull-Rom Tangents:
  Arrive: Points toward next (via prev-next vector)
  Leave: Points toward next (via prev-next vector)
  → Same direction, creates classic spline smoothness
```

**Neighbor Distance Effect:**

```
Close neighbors:
  P0 --- P1 --- P2
  Short tangents (proportional to distance)

Distant neighbors:
  P0 ---------- P1 ---------- P2
  Long tangents (proportional to distance)

Catmull-Rom automatically adjusts tangent length
based on neighbor spacing for consistent curvature.
```

Good for: smooth splines, interpolation curves, C1 continuity, balanced curvature, standard curve fitting

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

**Standard Spline Interpolation:**

```
[Path Points]
  ↓
[Write Tangents] Tangents: Catmull-Rom, Scales: 1.0
  ↓
[Points with Catmull-Rom tangents for classic smooth curves]
```

**Gentler Curves:**

```
[Path Points]
  ↓
[Write Tangents] Tangents: Catmull-Rom, Arrive Scale: 1.5, Leave Scale: 1.5
  ↓
[Points with longer tangents for wider, smoother curves]
```

**Tight Interpolation:**

```
[Path Points]
  ↓
[Write Tangents] Tangents: Catmull-Rom, Arrive Scale: 0.3, Leave Scale: 0.3
  ↓
[Points with short tangents staying close to control points]
```

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Tangents/PCGExTangentsCatmullRom.h)
