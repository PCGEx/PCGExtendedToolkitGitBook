---
description: Connects points with similar scalar values (isolines/contours).
icon: circle-dashed
---

# G-Probe : Level Set

### Overview

This global probe creates connections between points that have similar values for a specified scalar attribute. Points are connected only if their attribute values differ by less than the maximum level difference threshold. This creates isoline-like or contour-following connectivity, useful for connecting points at similar elevations, temperatures, densities, or any other scalar field.

<figure><img src="../../../../.gitbook/assets/image (295).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Level Reading**: Reads the scalar attribute value for each point
2. **Normalization**: Optionally normalizes values to 0-1 range for consistent comparison
3. **Level Matching**: For each point, finds neighbors within the search radius whose level differs by less than the threshold
4. **Connection Limiting**: Connects to up to MaxConnectionsPerPoint neighbors that satisfy the level criteria

**Usage Notes**

* **Contour Lines**: Using height (Z position) as the level attribute creates terrain contour connectivity
* **Normalization**: Enable when comparing attributes with varying ranges across datasets, ensuring consistent behavior
* **Connection Limit**: Controls density of connections; lower values create sparser, more linear contours

### Behavior

```
Level Set Connectivity (MaxLevelDifference=1):

    Points with Z values:          Connections:

       [5]   [4]   [3]               [5]───[4]   [3]
        │     │     │                 │           │
       [4]   [6]   [2]               [4]   [6]   [2]
        │     │     │                 │     │     │
       [3]   [5]   [1]               [3]   [5]   [1]

    Points at similar levels       Only points within
    (difference ≤ 1) connect       level tolerance connect


    Contour visualization:

    ┌─────────────────────┐
    │   5 ─── 5 ─── 5     │  Level 5 contour
    │   │           │     │
    │   4 ─── 4 ─── 4     │  Level 4 contour
    │   │           │     │
    │   3 ─── 3 ─── 3     │  Level 3 contour
    └─────────────────────┘
```

### Settings

#### Level Configuration

<details>

<summary><strong>Level Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The scalar attribute defining the "level" or field value for each point. Points with similar values for this attribute will be connected.

Default: `$Position.Z`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Level Difference</strong> <code>double</code></summary>

The maximum allowed difference in level values for two points to be connected. Smaller values create tighter, more precise contours; larger values allow more tolerance.

Default: `10.0`

Min: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normalize Levels</strong> <code>bool</code></summary>

When enabled, normalizes all level values to a 0-1 range before comparison. This makes the Max Level Difference work as a percentage of the total range rather than an absolute value.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Connections Per Point</strong> <code>int32</code></summary>

The maximum number of connections each point can make to neighbors within the level tolerance. Acts as a K-nearest filter among level-compatible neighbors.

Default: `4`

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

The maximum spatial distance to search for neighbors. Points must be both within this distance AND within the level tolerance to connect.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeLevelSet.h)
