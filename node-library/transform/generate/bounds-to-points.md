---
icon: circle
---

# Bounds To Points

### Overview

This node creates new points at specific positions on each input point's bounding box surface. Position is controlled using UVW coordinates, where each axis ranges from -1 to 1 (edges of bounds) with 0 at the center. Optional symmetry generates mirrored pairs across an axis, useful for creating matching points on opposite sides of bounds.

### How It Works

1. **Bounds Reading**: Gets the bounding box for each point using the specified bounds reference
2. **UVW Calculation**: Resolves UVW coordinates (constant or from attributes) to determine position within bounds
3. **Position Mapping**: Converts UVW coordinates to world-space positions on the bounds surface
4. **Symmetry Application**: If symmetry is enabled, generates an additional mirrored point across the specified axis
5. **Output Creation**: Creates points with optional custom extents and scale

**Usage Notes**

* **UVW Range**: Values range from -1 to 1, where 0 is the center. A value of 1 on any axis places the point at the positive edge of bounds, -1 at the negative edge
* **Surface Sampling**: UVW coordinates can come from attributes, allowing varied placement across different points
* **Symmetry Pairs**: When symmetry is enabled, each input point generates two output points mirrored across the chosen axis

### Behavior

**Bounds To Points:**

```
UVW Coordinate System:

                 W = 1
                   ↑
         ┌─────────┼─────────┐
         │         │         │
         │    ●────┼────●    │  ← V = 0
   U = -1│─────────┼─────────│U = 1
         │    ●────┼────●    │
         │         │         │
         └─────────┼─────────┘
                   ↓
                 W = -1

Example: UVW = (1, 0, 0) → point at center-right edge
         UVW = (0, 0, 1) → point at top center


Symmetry (Axis = X):

    Input:                      Output:
                                    • (U = -1)
        ┌───────┐                   │
        │   ●   │       →       ┌───┼───┐
        └───────┘                   │
                                    • (U = 1)

Generates mirrored points across the X axis
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

<summary><strong>Symmetry Axis</strong> <code>EPCGExMinimalAxis</code></summary>

When set, generates an additional point mirrored across the specified axis.

| Option   | Description                          |
| -------- | ------------------------------------ |
| **None** | No symmetry - single point per input |
| **X**    | Mirror across the X axis             |
| **Y**    | Mirror across the Y axis             |
| **Z**    | Mirror across the Z axis             |

Default: `None`

⚡ PCG Overridable

</details>

#### UVW Coordinates

<details>

<summary><strong>UVW</strong> <code>FPCGExUVW</code></summary>

Configuration for point placement within bounds using normalized coordinates.

//→ See TODO FPCGExUVW

⚡ PCG Overridable

</details>

#### Output Points

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

<summary><strong>As Multiplier</strong> <code>bool</code></summary>

When enabled, the Extents value multiplies the source bounds instead of replacing them.

Default: `false`

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

Copies source point attributes as tags on output collections.

📋 _Visible when Generate Per Point Data is enabled_

//→ See TODO FPCGExAttributeToTagDetails

</details>

### Outputs

| Pin     | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| **Out** | Points | Points positioned on the bounds surface |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/Bounds/PCGExBoundsToPoints.h)
