---
description: Checks points inclusion against path-like data (paths, splines, polygons).
icon: circle-dashed
---

# Filter : Inclusion (Path/Splines)

### Overview

This filter tests whether points are inside, outside, or on the boundary of shapes defined by path-like data. It supports splines, paths, and polygon data as boundary definitions. The filter projects both the test points and the boundary shapes onto a 2D plane for containment testing, with configurable tolerance for boundary detection. Multiple shapes can be tested together with options for handling overlapping regions.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Shape Preparation**: Collects and samples path/spline data into polygonal boundaries.
2. **2D Projection**: Projects points and boundaries onto the configured plane.
3. **Containment Test**: Performs point-in-polygon tests for each point against all shapes.
4. **Result Aggregation**: Combines results across multiple shapes using the configured pick mode.

**Usage Notes**

* **Closed Loops**: For containment tests, shapes should typically be closed loops.
* **Projection**: All testing happens in 2D after projection; choose the projection plane carefully.
* **Tolerance**: Adjust tolerance based on the scale of your shapes and desired boundary sensitivity.

### Behavior

**Check Types:**

```
Shape: Closed polygon boundary

Point A (center):    IsInside → Pass, IsOutside → Fail
Point B (on edge):   IsOn → Pass, IsInside → Fail, IsOutside → Fail
Point C (outside):   IsOutside → Pass, IsInside → Fail

Combined checks:
IsInsideOrOn:  A=Pass, B=Pass, C=Fail
IsOutsideOrOn: A=Fail, B=Pass, C=Pass
```

**Multiple Shapes:**

```
Pick Mode: All
Shape 1: Point inside
Shape 2: Point outside
Result: Fail (must be inside ALL)

Pick Mode: Closest
Shape 1 (farther): Point inside
Shape 2 (closer):  Point outside
Result: Fail (uses closest shape only)
```

### Inputs

| Pin               | Type           | Description                                 |
| ----------------- | -------------- | ------------------------------------------- |
| **Paths/Splines** | Points/Splines | Path-like data defining the test boundaries |

### Settings

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Configuration for projecting 3D points onto a 2D plane for containment testing. Defines the projection plane and axis mapping.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sample Inputs</strong> <code>EPCGExSplineSamplingIncludeMode</code></summary>

Which input shapes to include in testing.

| Option                | Description                                 |
| --------------------- | ------------------------------------------- |
| **All**               | Test against all connected shapes           |
| **Closed loops only** | Only test against closed/looping shapes     |
| **Open lines only**   | Only test against open (non-looping) shapes |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Check Type</strong> <code>EPCGExSplineCheckType</code></summary>

The type of spatial relationship to test.

| Option                | Description                                 |
| --------------------- | ------------------------------------------- |
| **Is Inside**         | Point must be strictly inside the shape     |
| **Is Inside or On**   | Point must be inside or on the boundary     |
| **Is Inside and On**  | Point must be inside AND touching boundary  |
| **Is Outside**        | Point must be strictly outside the shape    |
| **Is Outside or On**  | Point must be outside or on the boundary    |
| **Is Outside and On** | Point must be outside AND touching boundary |
| **Is On**             | Point must be on the boundary               |
| **Is not On**         | Point must not be on the boundary           |

Default: `Is Inside`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Pick</strong> <code>EPCGExSplineFilterPick</code></summary>

How to handle multiple overlapping shapes.

| Option      | Description                            |
| ----------- | -------------------------------------- |
| **Closest** | Use result from the closest shape only |
| **All**     | Must satisfy condition for all shapes  |

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Distance tolerance for determining whether a point is considered "on" the boundary.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spline Scales Tolerance</strong> <code>bool</code></summary>

When enabled, scales the tolerance by the spline's "thickness" (the length of its scale vector).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inclusion Offset</strong> <code>double</code></summary>

If non-zero, applies an offset (inset or expansion) to the shape boundaries before testing.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Min Inclusion Count</strong> <code>bool</code></summary>

When enabled, requires the point to be inside at least a minimum number of shapes.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Inclusion Count</strong> <code>int32</code></summary>

Minimum number of shapes the point must be inside to pass.

Default: `2`

⚡ PCG Overridable

📋 _Visible when Use Min Inclusion Count is enabled_

</details>

<details>

<summary><strong>Use Max Inclusion Count</strong> <code>bool</code></summary>

When enabled, limits the maximum number of shapes the point can be inside.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Inclusion Count</strong> <code>int32</code></summary>

Maximum number of shapes the point can be inside to pass.

Default: `10`

⚡ PCG Overridable

📋 _Visible when Use Max Inclusion Count is enabled_

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the result of the test.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Expand Z Axis</strong> <code>double</code></summary>

Expansion on Z axis for 3D tolerance. A value of -1 means infinite height (pure 2D test).

Default: `-1`

</details>

<details>

<summary><strong>Winding Mutation</strong> <code>EPCGExWindingMutation</code></summary>

Enforces a specific winding order for the path boundaries during testing.

Default: `Counter Clockwise`

</details>

<details>

<summary><strong>Fidelity</strong> <code>double</code></summary>

When sampling splines into polygons, defines the resolution. Lower values produce higher fidelity but slower execution.

Default: `50`

</details>

<details>

<summary><strong>Check Against Data Bounds</strong> <code>bool</code></summary>

When enabled with collection filter, uses collection bounds as a proxy point instead of per-point testing.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore Self</strong> <code>bool</code></summary>

When enabled, a collection will never be tested against itself.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy, Missing Data Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExInclusionFilter.h)
