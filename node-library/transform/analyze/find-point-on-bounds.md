---
description: Find the closest point on the dataset bounds.
icon: circle
---

# Find Point on Bounds

### Overview

This node identifies which input point is closest to a target position on the dataset's bounding box. The target position is specified using UVW coordinates relative to the bounds. This enables locating boundary points, finding extremities, or selecting points near specific edges or corners of a point cloud.

### How It Works

1. **Bounds Calculation**: Computes the overall bounding box of all input points
2. **Target Position**: Resolves the UVW coordinates to a world-space position on the bounds surface
3. **Distance Search**: Finds the point closest to the target position
4. **Offset Application**: Optionally offsets the target position away from the bounds center
5. **Result Output**: Outputs the closest point(s) with optional attribute carry-over

**Usage Notes**

* **Dataset-Level Operation**: The bounds encompass all points in the dataset, not individual point bounds
* **Best Fit Bounds**: When enabled, uses an oriented bounding box aligned to the data's principal axes rather than axis-aligned bounds
* **UVW Range**: Values typically range from -1 to 1, with corners at combinations like (1, 1, 1) or (-1, -1, 1)

### Behavior

**Find Point on Bounds:**

```
Dataset bounds with points:

     ┌─────────────────────┐ ← UVW (1, 1, 1)
     │  •     •      •     │
     │     •     •         │
     │  •        •    •    │
     │       ★ ← closest   │
     │  •  •      •        │
     └─────────────────────┘
          ↑
     Target position (UVW based)

UVW = (1, 0, 0) → right edge center
UVW = (-1, -1, -1) → bottom-left-back corner
UVW = (0, 0, 0) → bounds center


Best Fit Bounds:

    Axis-aligned:              Best fit (oriented):
    ┌────────────────┐         ╲────────────────╱
    │ ╲              │          ╲              ╱
    │  ╲  points     │    →      ╲  points    ╱
    │   ╲            │            ╲          ╱
    └────────────────┘             ╲────────╱

Best fit creates a tighter bounding box
aligned to the data's principal directions
```

### Settings

#### Output Configuration

<details>

<summary><strong>Output Mode</strong> <code>EPCGExPointOnBoundsOutputMode</code></summary>

How the results are output.

| Option                | Description                                             |
| --------------------- | ------------------------------------------------------- |
| **Merged Points**     | All closest points combined into a single collection    |
| **Per-point dataset** | Each input dataset outputs its closest point separately |

Default: `Merged Points`

⚡ PCG Overridable

</details>

#### Bounds Configuration

<details>

<summary><strong>Best Fit Bounds</strong> <code>bool</code></summary>

When enabled, uses an oriented bounding box aligned to the data's principal axes instead of an axis-aligned bounding box. Results in a tighter fit for non-axis-aligned point distributions.

Default: `false`

</details>

<details>

<summary><strong>Use Best Fit bounds axis</strong> <code>EPCGExAxisOrder</code></summary>

The axis priority order when computing best fit bounds. Determines which axis is considered primary for alignment.

| Option        | Description          |
| ------------- | -------------------- |
| **X > Y > Z** | X axis prioritized   |
| **Y > Z > X** | Y axis prioritized   |
| **Z > X > Y** | Z axis prioritized   |
| **Y > X > Z** | Y then X prioritized |
| **Z > Y > X** | Z then Y prioritized |
| **X > Z > Y** | X then Z prioritized |

Default: `Y > X > Z`

📋 _Visible when Best Fit Bounds is enabled_

</details>

#### Target Position

<details>

<summary><strong>UVW Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the UVW coordinates are constant or read from an attribute.

| Option        | Description                                |
| ------------- | ------------------------------------------ |
| **Constant**  | Use the constant UVW value specified below |
| **Attribute** | Read UVW from a point attribute            |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>UVW (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the UVW coordinates per point.

📋 _Visible when UVW Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>UVW</strong> <code>FVector</code></summary>

The UVW position of the target within bounds. Each component ranges from -1 to 1, representing position from one edge to the opposite edge.

Default: `(1, 1, 1)` (corner)

📋 _Visible when UVW Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Distance to offset the target position away from the bounds center. Positive values push outward.

Default: `1`

⚡ PCG Overridable

</details>

#### Attribute Handling

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are transferred from source points to the output.

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Quiet Attribute Mismatch Warning</strong> <code>bool</code></summary>

When enabled, suppresses warnings about attribute type mismatches during merging.

Default: `false`

</details>

### Outputs

| Pin     | Type   | Description                                           |
| ------- | ------ | ----------------------------------------------------- |
| **Out** | Points | The closest point(s) to the target position on bounds |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/Bounds/PCGExFindPointOnBounds.h)
