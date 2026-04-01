---
description: >-
  Connect points that pass the value comparison between the probing point and
  the candidate point.
icon: circle-dashed
---

# Probe : Numeric Compare

### Overview

This per-point probe creates connections based on comparing a numeric attribute between points. Only candidates whose attribute value satisfies the comparison condition (greater than, less than, equal, etc.) relative to the source point's value will be connected. This enables creating connections that follow value gradients or link points with specific numeric relationships.

### How It Works

1. **Attribute Reading**: Reads the specified numeric attribute for all points
2. **Candidate Search**: Finds candidates within the search radius
3. **Comparison Filtering**: For each candidate, compares its attribute value against the source point's value
4. **Distance Sorting**: Among passing candidates, sorts by distance
5. **Connection Limiting**: Creates up to MaxConnections edges to the closest passing candidates

**Usage Notes**

* **Comparison Direction**: "Strictly Greater" means connect to points with higher values than the source; use for uphill/ascending connections
* **Tolerance**: For NearlyEqual/NearlyNotEqual comparisons, the tolerance defines how close values must be
* **Value Gradients**: Combine with gradient-based probes for complex flow patterns

### Behavior

```
Numeric Compare (Attribute=Height, Comparison=StrictlyGreater):

    Points with height values:

       [5]   [8]   [3]
        │     │     │
       [4]   [6]   [7]
        │     │     │
       [2]   [9]   [1]

    From point [4], connects to neighbors with Height > 4:

       [5]───[8]   [3]
        ╲     │
       [4]   [6]───[7]
              │
       [2]   [9]   [1]

    Point [4] connects to [5], [6], [8] (all > 4)
    but not [2], [3], [1] (all < 4)
```

### Settings

#### Comparison Configuration

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The numeric attribute to compare between the source point and candidate points.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use when evaluating candidates.

| Option               | Description                          |
| -------------------- | ------------------------------------ |
| **Strictly Greater** | Candidate value > Source value       |
| **Strictly Smaller** | Candidate value < Source value       |
| **Equal**            | Candidate value == Source value      |
| **Not Equal**        | Candidate value != Source value      |
| **Greater Or Equal** | Candidate value >= Source value      |
| **Smaller Or Equal** | Candidate value <= Source value      |
| **Nearly Equal**     | Values within tolerance              |
| **Nearly Not Equal** | Values differ by more than tolerance |

Default: `Strictly Greater`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

The tolerance for "Nearly Equal" and "Nearly Not Equal" comparisons. Values within this range are considered equal.

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison = Nearly Equal or Nearly Not Equal_

⚡ PCG Overridable

</details>

#### Connection Limits

<details>

<summary><strong>Max Connections Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the maximum connections is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the limit from a point attribute  |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Connections</strong> <code>int32</code></summary>

The maximum number of connections per point among candidates that pass the comparison.

Default: `1`

Min: `0`

📋 _Visible when Max Connections Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Connections (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the maximum connections value per point.

📋 _Visible when Max Connections Input = Attribute_

⚡ PCG Overridable

</details>

#### Coincidence Prevention

<details>

<summary><strong>Prevent Coincidence</strong> <code>bool</code></summary>

When enabled, attempts to prevent selecting multiple connections in roughly the same direction.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Coincidence Prevention Tolerance</strong> <code>double</code></summary>

The tolerance for coincidence detection.

Default: `0.001`

Min: `0.00001`

📋 _Visible when Prevent Coincidence is enabled_

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

The maximum distance to search for neighbors.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExProbeNumericCompare.h)
