---
description: >-
  A tensor that samples nearby surfaces to compute direction fields. Uses all
  connected surface sources additively.
hidden: true
icon: circle-dashed
---

# Tensor : Surface

### Overview

The Surface Tensor computes directions based on nearby surface geometry. It can detect surfaces through world collision queries, actor references, or PCG surface data, then calculate directions that flow along, toward, away from, or orbit around those surfaces. This enables terrain-following flows, surface-avoidance patterns, and other geometry-aware tensor fields.

### How It Works

1. **Surface Detection**: Finds the nearest surface using configured sources (world collision, actor primitives, PCG surfaces).
2. **Hit Analysis**: Determines the closest point on the surface and its normal.
3. **Direction Computation**: Calculates the tensor direction based on the selected mode.
4. **Falloff Application**: Applies distance-based falloff from the maximum search distance.

**Usage Notes**

* **Multiple Sources**: Connect actor references, PCG surfaces, and/or enable world collision — all sources are checked additively.
* **Mode Selection**: Choose how the tensor relates to surfaces: flowing along them, pointing toward/away, following normals, or orbiting.
* **Missing Surfaces**: By default, sampling fails if no surface is found. Enable "Return Zero On Miss" for graceful degradation.

### Behavior

**Along Surface Mode:**

```
Surface:    ▄▄▄▄▄▄▄▄▄▄

Tensor:     → → → → → →

Projects reference direction onto surface tangent.
```

**Toward/Away Surface Mode:**

```
Surface:    ▄▄▄▄▄▄▄▄▄▄
            ↓ ↓ ↓ ↓ ↓ ↓  (Toward)
            ↑ ↑ ↑ ↑ ↑ ↑  (Away)

Points toward or away from nearest surface point.
```

**Orbit Mode:**

```
Surface:    ▄▄▄●▄▄▄▄▄▄
              ╱
            ←   (circular motion around surface)
              ╲

Cross product of normal and reference axis.
```

### Inputs

| Pin                  | Type         | Description                                                      |
| -------------------- | ------------ | ---------------------------------------------------------------- |
| **Actor References** | Point Data   | Points with actor reference attribute pointing to surface actors |
| **Surfaces**         | Surface Data | PCG surface data for surface detection                           |

### Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExSurfaceTensorMode</code></summary>

How to compute the tensor direction from the surface.

| Option                | Description                                                                |
| --------------------- | -------------------------------------------------------------------------- |
| **Along Surface**     | Project reference direction onto surface tangent plane                     |
| **Toward Surface**    | Direct toward nearest surface point                                        |
| **Away From Surface** | Direct away from nearest surface point                                     |
| **Surface Normal**    | Use the surface normal directly at the nearest point                       |
| **Orbit**             | Circular motion around the surface (cross product of normal and reference) |

Default: `Along Surface`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Distance</strong> <code>double</code></summary>

Maximum search distance for surface detection. Surfaces beyond this distance are not considered.

Default: `1000`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Reference Axis</strong> <code>EPCGExAxis</code></summary>

Reference axis from the probe transform used for Along Surface and Orbit modes. This axis is projected onto the surface tangent plane or crossed with the normal.

Default: `Forward`

📋 _Visible when Mode = Along Surface or Orbit_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Normal For Away</strong> <code>bool</code></summary>

When enabled, uses the surface normal for Away From Surface mode instead of the direction from the nearest point. Provides more consistent results on curved surfaces.

Default: `true`

📋 _Visible when Mode = Away From Surface_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use World Collision</strong> <code>bool</code></summary>

When enabled, uses world collision queries to find surfaces in addition to any connected inputs.

Default: `false`

</details>

<details>

<summary><strong>Collision Settings</strong> <code>FPCGExCollisionDetails</code></summary>

Collision settings for world collision queries including channel, object types, and trace complexity.

📋 _Visible when Use World Collision is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Actor Reference Attribute</strong> <code>FName</code></summary>

Name of the attribute containing actor reference paths when using Actor References input.

Default: `ActorReference`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Return Zero On Miss</strong> <code>bool</code></summary>

When enabled, returns zero direction when no surface is found instead of failing the sample. Useful for graceful degradation at field boundaries.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits from the tensor factory provider base.

→ See Tensor Factory for common tensor options.

### Outputs

| Pin        | Type           | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| **Tensor** | Tensor Factory | Tensor definition for use with tensor-consuming nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Tensors/PCGExTensorSurface.h)
