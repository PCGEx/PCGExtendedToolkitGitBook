---
description: Subdivide paths segments.
icon: circle
---

# Path : Subdivide

### Overview

This node increases path point density by adding intermediate points between existing points. Subdivision can be based on a target distance, a fixed count per segment, or Manhattan (axis-aligned) stepping. New points receive blended attributes from their neighboring original points.

### How It Works

1. **Evaluate Segments**: For each segment between consecutive points, determine how many subdivision points to add.
2. **Calculate Positions**: Compute positions for new points based on the subdivision method.
3. **Insert Points**: Add new points at the calculated positions.
4. **Blend Attributes**: Apply attribute blending from segment endpoints to new points.
5. **Write Metadata**: Optionally flag new points and write alpha values.

**Usage Notes**

* **Distance Mode**: Creates points at regular intervals along each segment. Longer segments get more points.
* **Count Mode**: Creates a fixed number of points per segment regardless of length.
* **Manhattan Mode**: Creates axis-aligned steps rather than straight interpolation, useful for grid-like paths.
* **Redistribute Evenly**: In Distance mode, adjusts spacing so points are evenly distributed rather than having a shorter final segment.

### Behavior

```
Subdivide by distance (distance = segment_length / 3):

Before:     ●─────────────────────●
            A                     B

After:      ●───────●───────●───────●
            A      new     new      B

Subdivide by count (count = 2):

Before:     ●───────────●───────────────────●
            A           B                   C

After:      ●───●───●───●───●───●───●───●───●
            A   +   +   B   +   +   +   +   C
            (2 points added per segment)
```

**Manhattan subdivision:**

```
            ●───────────┐
                        │
Before:                 └───────────●

            ●───●───●───┐
                        │
After:                  ●
                        │
                        └───●───●───●
            (axis-aligned steps)
```

### Inputs

| Pin         | Type             | Description                               |
| ----------- | ---------------- | ----------------------------------------- |
| **In**      | Points           | Path points to subdivide                  |
| **Filters** | Filter Factories | Filters for which segments are subdivided |
| **Labels**  | Points           | Optional labeling data                    |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Subdivide Method</strong> <code>EPCGExSubdivideMode</code></summary>

How subdivision points are calculated.

| Option        | Description                               |
| ------------- | ----------------------------------------- |
| **Distance**  | Add points at fixed distance intervals.   |
| **Count**     | Add a fixed number of points per segment. |
| **Manhattan** | Create axis-aligned step patterns.        |

Default: `Distance`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Amount Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the subdivision amount value.

| Option        | Description                                     |
| ------------- | ----------------------------------------------- |
| **Constant**  | Use the constant Distance or Count value.       |
| **Attribute** | Read subdivision amount from a point attribute. |

Default: `Constant`

📋 _Visible when Subdivide Method ≠ Manhattan_

</details>

<details>

<summary><strong>Distance</strong> <code>double</code></summary>

Target distance between points when using Distance mode.

Default: `10`

📋 _Visible when Subdivide Method = Distance and Amount Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Count</strong> <code>int32</code></summary>

Number of points to add per segment when using Count mode.

Default: `10`

📋 _Visible when Subdivide Method = Count and Amount Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Redistribute Evenly</strong> <code>bool</code></summary>

Adjust point spacing so all subdivision points are evenly distributed along the segment. When disabled, the last subdivision may be shorter than the target distance.

Default: `false`

📋 _Visible when Subdivide Method = Distance_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Manhattan</strong> <code>FPCGExManhattanDetails</code></summary>

Settings for Manhattan (axis-aligned) subdivision.

| Property            | Description                            |
| ------------------- | -------------------------------------- |
| **Method**          | Simple or custom Manhattan stepping.   |
| **Order**           | Axis priority order (X > Y > Z, etc.). |
| **Grid Size Input** | Constant or attribute for step size.   |
| **Space Align**     | World or local space alignment.        |

📋 _Visible when Subdivide Method = Manhattan_

⚡ PCG Overridable

</details>

***

#### Blending

<details>

<summary><strong>Blending</strong> <code>UPCGExSubPointsBlendInstancedFactory</code></summary>

How attributes are interpolated for new subdivision points.

| Option            | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| **Inherit Start** | New points inherit attributes from the segment's start point. |
| **Inherit End**   | New points inherit attributes from the segment's end point.   |
| **None**          | No attribute blending for new points.                         |

⚡ PCG Overridable

</details>

***

#### Additional Outputs

<details>

<summary><strong>Flag Sub Points</strong> <code>bool</code></summary>

Write a boolean attribute to mark which points were created by subdivision (vs original points).

Default: `false`

Attribute: `IsSubPoint`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Alpha</strong> <code>bool</code></summary>

Write the interpolation alpha (0-1) for each point along its segment.

* Original start point: `0` or `DefaultAlpha`
* Subdivision points: interpolated value
* Original end point: `1` or `DefaultAlpha`

Default: `false`

Attribute: `Alpha` · Default Value: `1`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExSubdivide.h)
