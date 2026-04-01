---
description: Reduce points while preserving path shape using tangents.
icon: circle
---

# Path : Reduce

### Overview

This node simplifies paths by removing points while computing arrive and leave tangents that preserve the original path's curvature. Unlike simple point removal, this node outputs tangent vectors that can be used to reconstruct smooth curves through the remaining points.

### How It Works

1. **Evaluate Points**: Analyze each point against the selected mode (Preserve or Anchor).
2. **Build Mask**: Determine which points to keep based on filters and error tolerance.
3. **Compute Tangents**: For each remaining point, calculate arrive and leave tangent vectors that approximate the original path shape.
4. **Apply Smoothing**: Optionally smooth tangent directions for more gradual transitions.
5. **Output**: Return reduced paths with tangent attributes.

**Usage Notes**

* **Preserve vs Anchor**: In Preserve mode, filtered points are guaranteed to stay while others may be removed based on error tolerance. In Anchor mode, only the filtered points remain, regardless of path shape.
* **Tangent Reconstruction**: The output tangents are designed for use with spline or Bezier curve systems that can reconstruct smooth curves from control points and tangent vectors.
* **Error Tolerance**: Higher values allow more aggressive point removal but may deviate further from the original path shape.

### Behavior

```
Path Reduction with tangent preservation:

Original path (8 points):
●───●───●───●───●───●───●───●

After Reduce (3 points with tangents):
●═══════════●═══════════●
 ↘          ↕           ↙
  (tangents preserve curve shape)

Arrive/Leave tangents at each point:
     ←─Arrive  Leave─→
           ↘  ↗
            ●
```

### Inputs

| Pin         | Type             | Description                                 |
| ----------- | ---------------- | ------------------------------------------- |
| **In**      | Points           | Path points to reduce                       |
| **Filters** | Filter Factories | Point filters for preserve/anchor selection |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExPathReduceFilterMode</code></summary>

How filters determine which points are kept.

| Option       | Description                                                                                     |
| ------------ | ----------------------------------------------------------------------------------------------- |
| **Preserve** | Filtered points are guaranteed to remain. Other points may be removed based on error tolerance. |
| **Anchor**   | Only filtered points remain. The path is reduced strictly to anchor points.                     |

Default: `Preserve`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Error Tolerance</strong> <code>double</code></summary>

Maximum allowed deviation from the original path shape when removing points. Higher values allow more aggressive simplification but may lose path detail.

Default: `10`

📋 _Visible when Mode = Preserve_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Arrive Name</strong> <code>FName</code></summary>

Attribute name for the arrive tangent vector (the direction entering each point).

Default: `ArriveTangent`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Leave Name</strong> <code>FName</code></summary>

Attribute name for the leave tangent vector (the direction exiting each point).

Default: `LeaveTangent`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smoothing Mode</strong> <code>EPCGExTangentSmoothing</code></summary>

How tangent vectors are smoothed for gradual transitions.

| Option             | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| **None**           | No smoothing. Tangents are computed directly from path geometry. |
| **Direction Only** | Smooth only the tangent direction, not the magnitude.            |
| **Full**           | Smooth both direction and magnitude for the smoothest results.   |

Default: `Full`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smoothing</strong> <code>double</code></summary>

Smoothing intensity (0-1). Higher values produce smoother tangent transitions.

* `0` - No smoothing effect
* `1` - Full smoothing

Can be read from an attribute for per-point control.

Default: `1.0`

📋 _Visible when Smoothing Mode ≠ None_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathReduce.h)
