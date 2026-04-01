---
description: Creates sequential chain connections based on sorting criteria.
icon: circle-dashed
---

# G-Probe : Chain/Path

### Overview

This global probe creates a single continuous path by connecting points in a sorted sequence. Points are first sorted according to the chosen mode, then connected sequentially to form a chain. The resulting graph is a path (or loop) where each point connects to at most two neighbors - the previous and next point in the sequence.

<figure><img src="../../../../.gitbook/assets/image (288).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Sort Points**: Orders all points according to the selected sorting mode (attribute value, axis projection, spatial curve, or Hilbert curve)
2. **Chain Creation**: Connects each point to the next point in the sorted order
3. **Loop Closure**: Optionally connects the last point back to the first to form a closed loop

**Usage Notes**

* **Global Probe**: This processes all points together to establish a single ordering, making it efficient for creating paths through point sets
* **Spatial Locality**: Hilbert curve sorting preserves spatial locality - nearby points in 3D space tend to be nearby in the chain
* **TSP Approximation**: The "By Spatial Curve" mode uses a greedy traveling salesman heuristic to minimize total path length, though it doesn't guarantee an optimal solution

### Behavior

```
Unsorted Points          By Attribute Sort        Resulting Chain
                              (density)
   3•     •5               1•     •2                1•─────•2
     •1                      •3                       │     │
  •2    •4                •4    •5                   •3     •5
                                                     │     │
                                                    •4─────┘

With bClosedLoop = true:    5 ──────────── 1
                              (closes loop)
```

### Settings

#### Sorting Configuration

<details>

<summary><strong>Sort Mode</strong> <code>EPCGExProbeChainSortMode</code></summary>

Determines how points are ordered before creating chain connections.

| Option                     | Description                                                              |
| -------------------------- | ------------------------------------------------------------------------ |
| **By Attribute**           | Sort by a scalar attribute value (ascending)                             |
| **By Axis Projection**     | Sort by each point's distance when projected onto an axis                |
| **By Spatial Curve (TSP)** | Greedy traveling salesman approximation - minimizes total path length    |
| **By Hilbert Curve**       | Sort by Hilbert curve index, preserving spatial locality in the ordering |

Default: `By Attribute`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to use for sorting when Sort Mode is "By Attribute". Points are ordered by ascending attribute value.

Default: `$Density`

📋 _Visible when Sort Mode = By Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Projection Axis</strong> <code>FVector</code></summary>

The axis to project points onto when Sort Mode is "By Axis Projection". Points are ordered by their distance along this direction from the origin.

Default: `(1, 0, 0)` (Forward)

📋 _Visible when Sort Mode = By Axis Projection_

⚡ PCG Overridable

</details>

#### Chain Options

<details>

<summary><strong>Closed Loop</strong> <code>bool</code></summary>

When enabled, creates an additional connection from the last point in the sequence back to the first point, forming a closed loop instead of an open path.

Default: `false`

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

The maximum distance to search for connections. For the chain probe, this limits which points can be chained together - if sequential points in the sort order are farther apart than this radius, they won't be connected.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeChain.h)
