---
description: >-
  Rotate a point or transform to closely match an input direction (or look at
  location) but preserve orthogonality.
icon: circle
---

# Best Match Axis

### Overview

This node adjusts point rotations so that one axis aligns as closely as possible with a target direction, while keeping the other axes perpendicular. Unlike a full "look at" rotation, this preserves the orthogonal relationship between axes, which is essential when you need aligned transforms without arbitrary roll or when matching orientations to surfaces or vectors.

### How It Works

1. **Target Resolution**: Determines the target direction based on mode (constant/attribute vector, world position, relative position, or closest target point)
2. **Direction Calculation**: For position-based modes, computes the look-at direction from each point toward the target
3. **Axis Matching**: Rotates the point's transform so that its best-matching axis aligns with the target direction
4. **Orthogonality Preservation**: Adjusts the rotation to maintain perpendicular relationships between all axes

**Usage Notes**

* **Orthogonal Constraint**: Unlike a free-form look-at, this node ensures axes remain perpendicular - useful for maintaining consistent orientations on grids or structured layouts
* **Closest Target Mode**: When using ClosestTarget mode, connect target points to the Labels input pin
* **Axis Selection**: The node automatically determines which axis best matches the target direction

### Behavior

#### Best Match Axis modes:

**Direction Mode:**

```
Target direction: ↗
                ↗
Point rotates to align best axis with direction
while keeping other axes orthogonal
```

**Look At (World Position):**

```
                            [Target Position]
                                ↑
[Point] ─────────────────────→ │

Computes direction from point to world position

Look At (Relative Position):
        [+Offset]
            ↑
[Point]──→│

Offset is relative to point's local space
```

**Closest Target:**

```
        [Target A]
            ↑
[Point]───→│    [Target B - farther]

Connects to nearest target from Labels input
```

### Inputs

| Pin        | Type   | Description                          |
| ---------- | ------ | ------------------------------------ |
| **In**     | Points | The input points to rotate           |
| **Labels** | Points | Target points for ClosestTarget mode |

### Settings

#### Target Mode

<details>

<summary><strong>Mode</strong> <code>EPCGExBestMatchAxisTargetMode</code></summary>

How the target direction is determined for axis matching.

| Option                          | Description                                                           |
| ------------------------------- | --------------------------------------------------------------------- |
| **Direction**                   | Best match against a direction vector                                 |
| **Look at Position (World)**    | Best match against the look at vector toward a world position         |
| **Look at Position (Relative)** | Best match against the look at vector toward a relative position      |
| **Look at Closest Target**      | Best match against the look at vector toward the closest target point |

Default: `Direction`

⚡ PCG Overridable

</details>

#### Direction Settings

<details>

<summary><strong>Match Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the match direction is a constant value or read from an attribute.

| Option        | Description                                |
| ------------- | ------------------------------------------ |
| **Constant**  | Use the constant direction specified below |
| **Attribute** | Read the direction from a point attribute  |

Default: `Attribute`

📋 _Visible when Mode is not ClosestTarget_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute or property to use as the target direction vector.

📋 _Visible when Mode is not ClosestTarget and Match Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match</strong> <code>FVector</code></summary>

The constant direction vector to match against.

Default: `(0, 0, 1)` (Up)

📋 _Visible when Mode is not ClosestTarget and Match Input = Constant_

⚡ PCG Overridable

</details>

#### Closest Target Settings

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Filters which targets get matched with which source points. Allows selective pairing based on tags or other criteria.

📋 _Visible when Mode = Look at Closest Target_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Distance Details</strong> <code>FPCGExDistanceDetails</code></summary>

Configuration for how distances are measured between source and target points.

| Property            | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| **Source**          | Distance measurement point on source (Center, Extents, etc.) |
| **Target**          | Distance measurement point on target                         |
| **Type**            | Distance calculation method (Euclidean, Manhattan, etc.)     |
| **Overlap Is Zero** | Whether overlapping bounds count as zero distance            |

📋 _Visible when Mode = Look at Closest Target_

⚡ PCG Overridable

</details>

### Outputs

| Pin     | Type   | Description                    |
| ------- | ------ | ------------------------------ |
| **Out** | Points | Points with adjusted rotations |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/Bounds/PCGExBestMatchAxis.h)
