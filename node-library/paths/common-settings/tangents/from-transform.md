---
description: >-
  Derives tangents from point transform orientation rather than neighbor
  positions.
icon: function
---

# From Transform

### Overview

This tangent factory calculates tangents based on each point's transform rotation, extracting a direction along a specified axis (forward, right, up, etc.). Both arrive and leave tangents use the same transform-derived direction, making this method independent of neighbor positions and suitable for points with meaningful rotations or for creating tangents aligned with specific local axes.

### How It Works

1. **Axis Selection**: Determines which transform axis to use (Forward, Backward, Right, Left, Up, Down)
2. **Rotation Extraction**: Retrieves the point's transform rotation
3. **Direction Calculation**: Extracts the direction vector along the selected axis from the rotation
4. **Direction Inversion**: Inverts the direction (multiplies by -1)
5. **Tangent Assignment**: Sets both arrive and leave tangents to the inverted direction
6. **Scaling Application**: Applies arrive and leave scale factors to the tangent vectors

**Usage Notes**

* **Transform-Based**: Unlike neighbor-based methods, this approach uses each point's rotation, making it independent of point positions or spacing.
* **Uniform Direction**: All points use their own transform's selected axis, meaning tangent directions can vary per point based on individual rotations.
* **Rotation Requirement**: Points must have meaningful rotations for this method to produce useful tangents. Random or default rotations may yield unpredictable results.
* **Edge Point Handling**: First and last points use the same calculation as intermediate points (no special neighbor-based logic needed).

### Behavior

```
Point with transform:
  Position: (100, 200, 0)
  Rotation: 45° around Z-axis
  Axis: Forward

Direction = Forward axis of rotation * -1
  = Transform's local forward direction (inverted)

Arrive tangent: Direction * ArriveScale
Leave tangent: Direction * LeaveScale

Both tangents aligned with point's forward axis (inverted).
```

**Axis Options:**

```
Axis = Forward:
  Tangent = Point's local forward direction (inverted)
  Default: Points along -Y in Unreal's coordinate system

Axis = Backward:
  Tangent = Point's local backward direction (inverted)
  Opposite of Forward

Axis = Right:
  Tangent = Point's local right direction (inverted)
  Points along local X-axis

Axis = Left:
  Tangent = Point's local left direction (inverted)
  Opposite of Right

Axis = Up:
  Tangent = Point's local up direction (inverted)
  Points along local Z-axis

Axis = Down:
  Tangent = Point's local down direction (inverted)
  Opposite of Up
```

**Transform Independence:**

```
Three points with different rotations:
  P0: Rotated 0°   (Forward = North)
  P1: Rotated 90°  (Forward = East)
  P2: Rotated 180° (Forward = South)

Axis = Forward:
  P0 tangent: South (0° forward inverted)
  P1 tangent: West  (90° forward inverted)
  P2 tangent: North (180° forward inverted)

Each point's tangent follows its own rotation,
independent of neighbors.
```

**Comparison with Neighbor-Based Methods:**

```
Auto/Catmull-Rom/From Neighbors:
  Tangent direction determined by neighbor positions
  → Path geometry drives tangent direction

From Transform:
  Tangent direction determined by point rotation
  → Point orientation drives tangent direction
```

**Scaling Effect:**

```
ArriveScale = 1.0, LeaveScale = 1.0:
  Standard transform-aligned tangents

ArriveScale = 2.0, LeaveScale = 2.0:
  Longer tangents along transform axis

ArriveScale = 0.5, LeaveScale = 0.5:
  Shorter tangents along transform axis

Different arrive/leave scales:
  Same direction, different magnitudes
```

**Use Cases:**

```
Oriented Points:
  Points with rotations from alignment/lookAt operations
  → Tangents follow point orientation

Directional Flow:
  Points representing flow direction (wind, water, etc.)
  → Tangents align with flow

Custom Orientation:
  User-defined rotations per point
  → Full control over tangent direction
```

Good for: transform-aligned tangents, rotation-based direction, oriented paths, custom tangent control, rotation-driven curves

### Settings

<details>

<summary><strong>Axis</strong> <code>EPCGExAxis</code></summary>

Which axis of the point's transform to use for tangent direction.

| Axis                  | Description                                 |
| --------------------- | ------------------------------------------- |
| **Forward** (default) | Point's local forward direction (inverted)  |
| **Backward**          | Point's local backward direction (inverted) |
| **Right**             | Point's local right direction (inverted)    |
| **Left**              | Point's local left direction (inverted)     |
| **Up**                | Point's local up direction (inverted)       |
| **Down**              | Point's local down direction (inverted)     |

The selected axis is extracted from the point's rotation and inverted (multiplied by -1) before being used as the tangent direction.

Default: `Forward`

</details>

#### Inherited Settings

The scaling parameters are defined in the parent context where this factory is used:

* **Arrive Scale Input**: Constant or Attribute source for arrive tangent scale
* **Arrive Scale**: Scale factor for arrive tangents (default: 1.0)
* **Leave Scale Input**: Constant or Attribute source for leave tangent scale
* **Leave Scale**: Scale factor for leave tangents (default: 1.0)

→ See Write Tangents for tangent scaling configuration.

### Practical Examples

**Oriented Path:**

```
[Points with meaningful rotations from lookAt/alignment]
  ↓
[Write Tangents] Tangents: From Transform, Axis: Forward
  ↓
[Tangents aligned with each point's forward direction]
```

**Directional Flow:**

```
[Points rotated to face flow direction]
  ↓
[Write Tangents] Tangents: From Transform, Axis: Forward
  ↓
[Tangents follow flow orientation]
```

**Custom Tangent Control:**

```
[Manually rotate points to desired tangent directions]
  ↓
[Write Tangents] Tangents: From Transform, Axis: Forward
  ↓
[Full control over tangent direction per point]
```

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Tangents/PCGExTangentsFromTransform.h)
