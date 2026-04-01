---
description: Applies transform variations to points with attribute override support.
icon: circle
---

# Transform Points

### Overview

This node applies randomized or attribute-driven variations to point transforms across position, rotation, and scale. Each transform component supports min/max ranges, per-component scaling, snapping to grids or angular increments, and can source values from constants or point attributes. The node provides the same variation controls as asset collection variations but with full PCG attribute override support.

### How It Works

1. **Value Sourcing**: For each transform parameter, reads from constant or attribute
2. **Range Application**: Applies min/max ranges (random per point if different)
3. **Scaling**: Multiplies by scaling factors
4. **Snapping** (optional): Snaps offset or result to grid/angle increments
5. **Transform Composition**: Combines offset, rotation, and scale variations
6. **Output**: Writes transformed points with updated transforms

### Behavior

**Position Offset:**

```
OffsetMin: (-10, -10, 0)
OffsetMax: (10, 10, 0)

Point transforms randomly within ±10 units on X/Y axes

Per-point random example:
Point 0: Offset = (-5.2, 3.7, 0)
Point 1: Offset = (8.1, -2.4, 0)
Point 2: Offset = (-1.3, 9.6, 0)
```

**Position Snapping:**

```
OffsetMin: (-100, -100, 0)
OffsetMax: (100, 100, 0)
SnapPosition: SnapResult
OffsetSnap: (50, 50, 0)

Random offset: (37, -62, 0)
Snapped result: (50, -50, 0) ← Nearest 50-unit grid point
```

**Absolute vs Local Offset:**

```
Point at (100, 200, 0) rotated 45°
Offset: (10, 0, 0)

AbsoluteOffset = false (local):
  Offset applied in point's local space
  → Moves 10 units along point's forward (rotated)

AbsoluteOffset = true (world):
  Offset applied in world space
  → Moves 10 units along world X axis
```

**Rotation Variation:**

```
RotationMin: (0, 0, -45)
RotationMax: (0, 0, 45)

Each point randomly rotated -45° to +45° on Z axis

bResetRotation = true:
  Rotation starts at (0,0,0) before variation

bResetRotation = false:
  Variation added to existing rotation
```

**Rotation Snapping:**

```
RotationMin: (0, 0, 0)
RotationMax: (0, 0, 360)
SnapRotation: SnapResult
RotationSnap: (90, 90, 90)

Random: (0, 0, 127)
Snapped: (0, 0, 90) ← Nearest 90° increment
```

**Scale Variation:**

```
ScaleMin: (0.8, 0.8, 0.8)
ScaleMax: (1.2, 1.2, 1.2)

Each point scaled 80% to 120%

UniformScale = true:
  Uses only X component for all axes
  → (1.1, 1.1, 1.1)

UniformScale = false:
  Independent per axis
  → (0.9, 1.15, 1.05)
```

**Scaling Multipliers:**

```
OffsetMin: (-10, -10, 0)
OffsetMax: (10, 10, 0)
OffsetScaling: (2, 2, 1)

Effective range:
Min: (-20, -20, 0)
Max: (20, 20, 0)

Useful for attribute-driven variation scale
```

**Attribute Override:**

```
OffsetMin:
  Input: Attribute
  Attribute: $CustomOffsetMin

Per-point offset mins from attribute values
Point 0: $CustomOffsetMin = (-5, -5, 0) → Uses this
Point 1: $CustomOffsetMin = (-20, -10, 0) → Uses this
```

**Snap Modes:**

```
SnapOffset:
  Snaps the random offset before applying
  Random: (37, -62, 0)
  Snapped offset: (50, -50, 0)
  Applied to point

SnapResult:
  Applies offset, then snaps final position
  Point: (100, 200, 0)
  Offset: (37, -62, 0)
  Result: (137, 138, 0)
  Snapped: (150, 150, 0)
```

Good for: transform variation, randomization, procedural placement, attribute-driven transforms

### Settings

#### Position

<details>

<summary><strong>Offset Min</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Minimum random offset per axis.

Can be constant or per-point attribute.

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset Max</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Maximum random offset per axis.

Can be constant or per-point attribute.

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset Scaling</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Scale applied to both Offset Min and Offset Max.

Multiplies the effective range without changing the base values.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Snap Position</strong> <code>EPCGExVariationSnapping</code></summary>

How to snap the random offset.

| Mode               | Description                                |
| ------------------ | ------------------------------------------ |
| **None** (default) | No snapping                                |
| **Snap Offset**    | Snap offset value before applying to point |
| **Snap Result**    | Snap final position after applying offset  |

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset Snap</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Grid size for offset snapping.

Defines snap increment per axis.

📋 _Visible when Snap Position != None_

Default: `(100, 100, 100)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute Offset</strong> <code>FPCGExInputShorthandSelectorBoolean</code></summary>

Apply offset in world space instead of local space.

* **false** (default): Local space (respects point rotation)
* **true**: World space (absolute direction)

Default: `false`

⚡ PCG Overridable

</details>

#### Rotation

<details>

<summary><strong>Reset Rotation</strong> <code>bool</code></summary>

Reset rotation to zero before applying variation.

* **false** (default): Add to existing rotation
* **true**: Start from zero rotation

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation Min</strong> <code>FPCGExInputShorthandSelectorRotator</code></summary>

Minimum random rotation per axis (degrees).

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation Max</strong> <code>FPCGExInputShorthandSelectorRotator</code></summary>

Maximum random rotation per axis (degrees).

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation Scaling</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Scale applied to both Rotation Min and Rotation Max.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Snap Rotation</strong> <code>EPCGExVariationSnapping</code></summary>

How to snap the random rotation.

| Mode               | Description                        |
| ------------------ | ---------------------------------- |
| **None** (default) | No snapping                        |
| **Snap Offset**    | Snap rotation before applying      |
| **Snap Result**    | Snap final rotation after applying |

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rotation Snap</strong> <code>FPCGExInputShorthandSelectorRotator</code></summary>

Angular increment for rotation snapping (degrees).

Common values: (90, 90, 90) for 90° snapping, (45, 45, 45) for 45° snapping.

📋 _Visible when Snap Rotation != None_

Default: `(90, 90, 90)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute Rotation</strong> <code>uint8 (bitmask)</code></summary>

Replace rotation instead of adding to it on selected axes.

Bitmask flags for Roll, Pitch, Yaw axes.

When enabled for an axis, sets that rotation component instead of adding.

</details>

#### Scale

<details>

<summary><strong>Reset Scale</strong> <code>bool</code></summary>

Reset scale to one before applying variation.

* **false** (default): Multiply existing scale
* **true**: Start from (1,1,1) scale

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale Min</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Minimum random scale per axis.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale Max</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Maximum random scale per axis.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale Scaling</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Scale applied to both Scale Min and Scale Max.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Uniform Scale</strong> <code>FPCGExInputShorthandSelectorBoolean</code></summary>

Use same scale on all axes (X component only).

* **false** (default): Independent per-axis scaling
* **true**: Use X component for all axes

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Snap Scale</strong> <code>EPCGExVariationSnapping</code></summary>

How to snap the random scale.

| Mode               | Description                          |
| ------------------ | ------------------------------------ |
| **None** (default) | No snapping                          |
| **Snap Offset**    | Snap scale variation before applying |
| **Snap Result**    | Snap final scale after applying      |

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale Snap</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Increment for scale snapping.

Example: (0.1, 0.1, 0.1) snaps to 0.1 increments.

📋 _Visible when Snap Scale != None_

Default: `(0.1, 0.1, 0.1)`

⚡ PCG Overridable

</details>

#### Extras

<details>

<summary><strong>Apply Scale to Bounds</strong> <code>bool</code></summary>

Apply the new scale to point bounds as well.

Updates the point's bounding box to match the new scale.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Reset Point Center</strong> <code>bool</code></summary>

Reposition point center within its bounds.

Allows offsetting the point's pivot relative to its bounds.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Point Center Location</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Bounds-relative coordinate used for the new center.

(0.5, 0.5, 0.5) = center, (0,0,0) = min corner, (1,1,1) = max corner.

📋 _Visible when Reset Point Center = true_

Default: `(0.5, 0.5, 0.5)`

⚡ PCG Overridable

</details>

### Input Shorthand Selectors

All transform parameters use input shorthand selectors that support:

**Input Types:**

* **Constant**: Fixed value for all points
* **Attribute**: Per-point values from point attributes

This allows complete flexibility in sourcing variation parameters from either constants or point data.

### Inputs

| Pin                    | Type             | Description                  |
| ---------------------- | ---------------- | ---------------------------- |
| **In**                 | Point Data       | Points to transform          |
| **Filters** (optional) | Filter Factories | Point filters (when enabled) |

### Outputs

| Pin     | Type       | Description        |
| ------- | ---------- | ------------------ |
| **Out** | Point Data | Transformed points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/PCGExTransformPoints.h)
