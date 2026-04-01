---
description: Output normalized position against data bounds to a new vector attribute.
icon: circle
---

# Normalize

### Overview

Normalize computes each point's position as a normalized coordinate (0-1) within the overall data bounds and writes the result to an attribute. This maps 3D world positions into a unit cube representation, useful for UV-like operations, gradient mapping, or spatial analysis relative to the point cloud's extent.

### How It Works

1. **Bounds Calculation**: Computes the overall bounding box of all input points.
2. **Position Mapping**: For each point, calculates its normalized position within bounds (0 = min bound, 1 = max bound).
3. **Transform Application**: Optionally applies a transform before normalization.
4. **Tiling & Offset**: Applies tiling multiplier and offset to the normalized coordinates.
5. **Wrapping**: Handles out-of-range values according to the wrapping mode.
6. **Output**: Writes the normalized vector to the specified attribute.

### Behavior

```
Data Bounds: Min(0,0,0) to Max(100,100,50)

Point at (50, 25, 50):
  Normalized = (0.5, 0.25, 1.0)

With Tile (2,2,2) and Offset (0.1, 0, 0):
  Result = (1.1, 0.5, 2.0) → Wrapped based on mode
```

### Inputs

| Pin        | Type       | Description                                                                                         |
| ---------- | ---------- | --------------------------------------------------------------------------------------------------- |
| **In**     | Point Data | Points to normalize                                                                                 |
| **Bounds** | Point Data | Optional external bounds reference (if provided, uses these bounds instead of computing from input) |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which point property to use when calculating the overall data bounds.

| Option             | Description                          |
| ------------------ | ------------------------------------ |
| **Scaled Bounds**  | Use point bounds multiplied by scale |
| **Density Bounds** | Use density-based bounds             |
| **Bounds**         | Use raw point bounds                 |
| **Center**         | Use point center positions only      |

Default: `Center`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>FVector</code></summary>

Offset applied to the normalized position after calculation.

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tile</strong> <code>FVector</code></summary>

Tiling factor applied to the normalized position, scaling the 0-1 range.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Wrapping</strong> <code>EPCGExIndexSafety</code></summary>

How to handle values outside the 0-1 range after tiling and offset.

| Option     | Description                                |
| ---------- | ------------------------------------------ |
| **Ignore** | Keep values as-is, even if outside 0-1     |
| **Tile**   | Wrap values using modulo (1.2 becomes 0.2) |
| **Clamp**  | Clamp values to 0-1 range                  |
| **Yoyo**   | Ping-pong values (1.2 becomes 0.8)         |

Default: `Tile`

⚡ PCG Overridable

</details>

<details>

<summary><strong>One Minus</strong> <code>uint8</code> (bitmask)</summary>

Which components should be inverted (1 - value). Useful for flipping axes.

Default: `0` (none)

</details>

<details>

<summary><strong>Transform Input</strong> <code>EPCGExInputValueType</code></summary>

Whether to read the pre-normalization transform from a constant or attribute.

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use a fixed transform            |
| **Attribute** | Read transform from an attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform</strong> <code>FTransform</code></summary>

Transform applied to positions before normalization.

Default: `Identity`

📋 _Visible when Transform Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to write the normalized position vector to.

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                     |
| ------- | ---------- | ----------------------------------------------- |
| **Out** | Point Data | Points with normalized position attribute added |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/PCGExNormalize.h)
