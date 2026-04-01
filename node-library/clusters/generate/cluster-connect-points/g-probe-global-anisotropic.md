---
description: Ellipsoidal distance metric for directional connectivity.
icon: circle-dashed
---

# G-Probe : Global Anisotropic

### Overview

This global probe finds connections using an anisotropic (direction-dependent) distance metric. Instead of measuring simple Euclidean distance, it scales distances differently along three perpendicular axes, creating an ellipsoidal search space. This allows connections to be preferentially established along certain directions while still considering neighbors in all directions.

<figure><img src="../../../../.gitbook/assets/image (293).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Axis Setup**: Establishes a coordinate frame from primary and secondary axes (tertiary is computed as their cross product)
2. **Distance Transform**: For each point pair, measures their separation and scales it by the axis factors
3. **K-Nearest Search**: Finds the K nearest neighbors using this transformed distance metric
4. **Connection Creation**: Creates edges to the K closest points in anisotropic space

**Usage Notes**

* **Global Probe**: This is a global probe that processes all points together rather than per-point, enabling optimized batch processing
* **Ellipsoidal Distance**: Lower scale values make an axis "shorter" in the search ellipsoid, preferring connections along that direction. Higher values make an axis "longer," penalizing connections in that direction.
* **Per-Point Normals**: When enabled, each point uses its own normal as the primary axis, allowing the directional preference to vary across the point set

### Behavior

```
Standard Euclidean (sphere)      Anisotropic (ellipsoid)
         │                              │
    ╭────┼────╮                    ╭────┼────╮
   ╱     │     ╲                  ╱     │     ╲
  │      │      │                │      │      │
──┼──────●──────┼──            ──┼──────●──────┼──
  │      │      │                │      │      │
   ╲     │     ╱                  ╲     │     ╱
    ╰────┼────╯                    ╰────┴────╯
         │                           flattened

Primary Scale < Secondary/Tertiary = prefer connections along primary axis
```

### Settings

#### Axis Configuration

<details>

<summary><strong>Primary Axis</strong> <code>FVector</code></summary>

The preferred connection direction. Connections along this axis will be favored when Primary Scale is lower than other scales.

Default: `(1, 0, 0)` (Forward)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary Axis</strong> <code>FVector</code></summary>

The cross direction, perpendicular to the primary axis. The tertiary axis is automatically computed as the cross product of primary and secondary.

Default: `(0, 1, 0)` (Right)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Per-Point Normal</strong> <code>bool</code></summary>

When enabled, uses each point's normal vector as the primary axis instead of a global direction. This allows the directional preference to follow local surface orientation.

Default: `false`

⚡ PCG Overridable

</details>

#### Scale Factors

<details>

<summary><strong>Primary Scale</strong> <code>double</code></summary>

Scale factor for distances along the primary axis. Lower values favor connections in this direction (distance appears shorter). Higher values penalize connections in this direction.

Default: `1.0`

Min: `0.1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary Scale</strong> <code>double</code></summary>

Scale factor for distances along the secondary axis.

Default: `2.0`

Min: `0.1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tertiary Scale</strong> <code>double</code></summary>

Scale factor for distances along the tertiary axis (computed as cross product of primary and secondary).

Default: `2.0`

Min: `0.1`

⚡ PCG Overridable

</details>

#### Neighbor Count

<details>

<summary><strong>K</strong> <code>int32</code></summary>

The number of nearest neighbors to connect to, measured using the anisotropic distance metric.

Default: `5`

Min: `1`

⚡ PCG Overridable

</details>

#### Search Radius (Inherited)

<details>

<summary><strong>Search Radius Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the search radius is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the radius from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Search Radius</strong> <code>double</code></summary>

The maximum distance to search for connections. Points beyond this distance are not considered as candidates.

Default: `100`

📋 _Visible when Search Radius Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Additional offset added to the search radius value.

Default: `0`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory to connect to Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeAnisotropic.h)
