---
description: Generate a two-point from a bound axis.
icon: circle
---

# Bounds Axis To Points

### Overview

This node extracts one axis from each point's bounding box and creates two points along that axis. The axis selection can be based on length (shortest, longest, or median) with additional constraints for direction alignment and size thresholds. This enables creating directional markers, axis-aligned segments, or extracting orientation information from point bounds.

### How It Works

1. **Bounds Reading**: Gets the bounding box for each point using the specified bounds reference
2. **Axis Priority**: Determines the initial axis based on priority (shortest, longest, or median length)
3. **Constraint Application**: Applies direction and size constraints to shift axis selection if needed
4. **Point Generation**: Creates two points at positions along the selected axis, offset by the U factor
5. **Output Creation**: Outputs points with optional custom extents and scale

**Usage Notes**

* **Axis Selection**: Priority sets the initial preference, but constraints can override it - for example, "Shortest but avoid Up" may select the second-shortest if the shortest points upward
* **U Factor**: Controls how far along the axis the points are placed; U=1 places points at the full extent, U=0.5 at half distance
* **Per-Point Data**: When enabled, each input point generates a separate output collection, useful for maintaining per-point associations

### Behavior

**Bounds Axis To Points:**

```
Input point with bounds:        Output (Priority=Shortest):

        ┌────────────┐                      •
        │            │                      │ (shortest axis)
        │     ●      │        →             ●
        │            │                      │
        └────────────┘                      •

Axis selection with constraints:

Priority: Shortest    Direction: Avoid Up    Size: Greater than 0.1

Bounds:   X=2, Y=5, Z=0.5
            ↓
Shortest: Z (0.5)
            ↓
Avoid Up: Z points up → shift to X (next shortest)
            ↓
Size > 0.1: X (2) passes threshold
            ↓
Selected: X axis
```

### Settings

#### Output Configuration

<details>

<summary><strong>Generate Per Point Data</strong> <code>bool</code></summary>

When enabled, generates a separate point collection for each input point. When disabled, all generated points go into a single collection.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bounds Reference</strong> <code>EPCGExPointBoundsSource</code></summary>

Which point property to use for bounds calculation.

| Option             | Description                          |
| ------------------ | ------------------------------------ |
| **Scaled Bounds**  | Use bounds multiplied by point scale |
| **Density Bounds** | Use the density-based bounds         |
| **Bounds**         | Use raw bounds without scale         |
| **Center**         | Use point center only (zero extents) |

Default: `Scaled Bounds`

⚡ PCG Overridable

</details>

#### Axis Selection

<details>

<summary><strong>Priority</strong> <code>EPCGExBoundAxisPriority</code></summary>

Which axis to initially select based on its length.

| Option       | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| **Shortest** | Select the axis with the smallest length                     |
| **Longest**  | Select the axis with the largest length                      |
| **Median**   | Select the middle-length axis (neither shortest nor longest) |

Default: `Shortest`

⚡ PCG Overridable

</details>

#### Direction Constraint

<details>

<summary><strong>Direction Constraint</strong> <code>EPCGExAxisDirectionConstraint</code></summary>

Shifts the axis selection based on whether the selected axis aligns with a reference direction.

| Option    | Description                                                    |
| --------- | -------------------------------------------------------------- |
| **None**  | No direction constraint                                        |
| **Avoid** | Shift away from axes that point toward the reference direction |
| **Favor** | Prefer axes that point toward the reference direction          |

Default: `Avoid`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>FVector</code></summary>

The reference direction for the direction constraint.

Default: `(0, 0, 1)` (Up)

📋 _Visible when Direction Constraint is not None_

⚡ PCG Overridable

</details>

#### Size Constraint

<details>

<summary><strong>Size Constraint</strong> <code>EPCGExAxisSizeConstraint</code></summary>

Shifts the axis selection based on whether its length passes a size threshold.

| Option      | Description                                        |
| ----------- | -------------------------------------------------- |
| **None**    | No size constraint                                 |
| **Greater** | Prefer axes with length greater than the threshold |
| **Smaller** | Prefer axes with length smaller than the threshold |

Default: `Greater`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold</strong> <code>double</code></summary>

The size threshold for the size constraint.

Default: `0.1`

📋 _Visible when Size Constraint is not None_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Constraints Order</strong> <code>EPCGExAxisConstraintSorting</code></summary>

Determines which constraint takes precedence when both direction and size constraints are active.

| Option                     | Description                                 |
| -------------------------- | ------------------------------------------- |
| **Size matters more**      | Apply size constraint first, then direction |
| **Direction matters more** | Apply direction constraint first, then size |

Default: `Direction matters more`

📋 _Visible when both Direction Constraint and Size Constraint are not None_

⚡ PCG Overridable

</details>

#### Output Points

<details>

<summary><strong>U</strong> <code>double</code></summary>

Extent factor at which the points will be created along the selected axis. A value of 1 places points at the full bounds extent; 0.5 places them at half distance.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Set Extents</strong> <code>bool</code></summary>

When enabled, sets the output points' extents to a custom value.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Extents</strong> <code>FVector</code></summary>

The extents to apply to output points.

Default: `(0.5, 0.5, 0.5)`

📋 _Visible when Set Extents is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Set Scale</strong> <code>bool</code></summary>

When enabled, sets the output points' scale to a custom value.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale</strong> <code>FVector</code></summary>

The scale to apply to output points.

Default: `(1, 1, 1)`

📋 _Visible when Set Scale is enabled_

⚡ PCG Overridable

</details>

#### Attribute Forwarding

<details>

<summary><strong>Point Attributes To Output Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copies source point attributes as tags on output collections. Only available when Generate Per Point Data is enabled.

📋 _Visible when Generate Per Point Data is enabled_

//→ See TODO FPCGExAttributeToTagDetails

</details>

### Outputs

| Pin     | Type   | Description                                       |
| ------- | ------ | ------------------------------------------------- |
| **Out** | Points | Two-point outputs aligned along the selected axis |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/Bounds/PCGExBoundsAxisToPoints.h)
