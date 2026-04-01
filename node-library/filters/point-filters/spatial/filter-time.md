---
description: Checks point positions against a path/spline/polygon closest alpha.
icon: circle-dashed
---

# Filter : Time

### Overview

This filter evaluates points by finding the closest position on connected path-like data (paths, splines, or polygons) and comparing the resulting normalized time value (alpha, ranging from 0 to 1 along the path) against a threshold. When multiple paths are connected, the filter can use the closest path or consolidate time values across all matching paths. This enables filtering based on where a point falls relative to the progression of a path.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Path Preparation**: Samples the connected path/spline data into polygonal shapes at the specified fidelity.
2. **Closest Point**: For each point, finds the closest position on the path(s) and computes the normalized time (0 at the start, 1 at the end).
3. **Consolidation**: If multiple paths are connected and Pick is set to All, consolidates time values using min, max, or average.
4. **Comparison**: Compares the resulting time value against Operand B using the selected comparison operator.
5. **Result**: Returns pass if the comparison succeeds, fail otherwise.

**Usage Notes**

* **Time Value**: The time (alpha) is a 0-to-1 value representing how far along the path the closest point falls. 0 is the start, 1 is the end.
* **Multiple Paths**: With the Closest pick mode, only the nearest path is considered. With All, time values from all paths are consolidated before comparison.
* **Collection Evaluation**: When Check Against Data Bounds is enabled, the collection's bounding box center is used as a proxy point instead of testing each point individually.

### Behavior

**Time Comparison:**

```
Path: [Start ─────────────── End]
      0.0                     1.0

Comparison: >= 0.5

Point A (near start): time=0.2 → 0.2 >= 0.5: Fail
Point B (midway):     time=0.5 → 0.5 >= 0.5: Pass
Point C (near end):   time=0.8 → 0.8 >= 0.5: Pass
```

**Multiple Paths with Consolidation:**

```
Path 1: closest time = 0.3
Path 2: closest time = 0.7

Consolidation = Min:     0.3 → compared against threshold
Consolidation = Max:     0.7 → compared against threshold
Consolidation = Average: 0.5 → compared against threshold
```

### Inputs

| Pin               | Type           | Description                                   |
| ----------------- | -------------- | --------------------------------------------- |
| **Paths/Splines** | Points/Splines | Path-like data to compute time values against |

### Settings

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

<summary><strong>Pick</strong> <code>EPCGExSplineFilterPick</code></summary>

When a point matches multiple paths, determines which result to use.

| Option      | Description                                     |
| ----------- | ----------------------------------------------- |
| **Closest** | Use the time from the nearest path only         |
| **All**     | Consolidate time values from all matching paths |

Default: `Closest`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Time Consolidation</strong> <code>EPCGExSplineTimeConsolidation</code></summary>

How to consolidate time values when multiple paths match.

| Option      | Description                        |
| ----------- | ---------------------------------- |
| **Min**     | Use the smallest time value        |
| **Max**     | Use the largest time value         |
| **Average** | Use the average of all time values |

Default: `Min`

⚡ PCG Overridable

📋 _Visible when Pick = All_

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use between the computed time and Operand B.

| Option   | Description                          |
| -------- | ------------------------------------ |
| **==**   | Strictly equal                       |
| **!=**   | Strictly not equal                   |
| **>=**   | Equal or greater                     |
| **<=**   | Equal or smaller                     |
| **>**    | Strictly greater                     |
| **<**    | Strictly smaller                     |
| **\~=**  | Nearly equal (within tolerance)      |
| **!\~=** | Nearly not equal (outside tolerance) |

Default: `Nearly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant value or read Operand B from an attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use the specified constant value      |
| **Attribute** | Read the value from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand B (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read Operand B from. Value is converted to double.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Operand B</strong> <code>float</code></summary>

The constant value to compare the time against.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Near-equality tolerance for approximate comparison modes.

Default: `DBL_COMPARE_TOLERANCE`

⚡ PCG Overridable

📋 _Visible when Comparison is Nearly Equal or Nearly Not Equal_

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the result of the filter.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Winding Mutation</strong> <code>EPCGExWindingMutation</code></summary>

Lets you enforce a path winding direction for testing.

| Option               | Description                     |
| -------------------- | ------------------------------- |
| **Unchanged**        | Use the path's original winding |
| **Clockwise**        | Force clockwise winding         |
| **CounterClockwise** | Force counter-clockwise winding |

Default: `Unchanged`

</details>

<details>

<summary><strong>Fidelity</strong> <code>double</code></summary>

When sampling splines into polygons, defines the resolution. Lower values produce higher fidelity but slower execution.

Default: `50`

</details>

<details>

<summary><strong>Check Against Data Bounds</strong> <code>bool</code></summary>

When enabled and used with a collection filter, uses collection bounds as a proxy point instead of per-point testing.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExTimeFilter.h)
