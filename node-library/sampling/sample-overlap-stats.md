---
description: Sample and write per-point overlap stats between entire point data.
icon: circle
---

# Sample : Overlap Stats

### Overview

This node analyzes overlap between points across multiple collections and writes overlap statistics to individual points. Unlike Discard By Overlap which operates on entire datasets, this node computes overlap metrics for each point, allowing fine-grained overlap-based filtering or weighting in downstream operations.

### How It Works

1. **Build Octrees**: Creates spatial indices for all point bounds.
2. **Detect Overlaps**: Tests bounds intersection between collections.
3. **Count Per Point**: Tracks how many other collections overlap each point.
4. **Write Stats**: Outputs overlap counts and relative metrics to attributes.

**Usage Notes**

* **Per-Point Stats**: Each point gets its own overlap count, not collection-wide.
* **Overlap Count**: Number of different collections that overlap this point.
* **Sub Count**: Total number of points from other collections that overlap.
* **Relative Values**: Counts relative to the maximum across all collections.

### Behavior

**Overlap Statistics:**

```
Collections A, B, C with overlapping regions:

Point P1 in Collection A:
   Overlaps with: 2 points from B, 1 point from C
   → OverlapCount = 2 (B and C)
   → OverlapSubCount = 3 (total overlapping points)

Point P2 in Collection A:
   Overlaps with: 0 points
   → OverlapCount = 0
   → OverlapSubCount = 0
```

### Inputs

| Pin               | Type   | Description                                   |
| ----------------- | ------ | --------------------------------------------- |
| **In**            | Points | Multiple point collections to analyze         |
| **Point Filters** | Params | Optional filters for which points participate |

### Settings

#### Overlap Detection

<details>

<summary><strong>Test Mode</strong> <code>EPCGExOverlapTestMode</code></summary>

How to test for overlaps between points.

| Option     | Description                                  |
| ---------- | -------------------------------------------- |
| **Fast**   | Only test using datasets' overall bounds     |
| **Box**    | Test every point's bounds as transformed box |
| **Sphere** | Test every point's bounds as spheres         |

Default: `Sphere`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use for overlap testing.

| Option             | Description                |
| ------------------ | -------------------------- |
| **Scaled Bounds**  | Bounds multiplied by scale |
| **Density Bounds** | Density-based bounds       |
| **Bounds**         | Raw point bounds           |
| **Center**         | Point center only          |

Default: `Scaled Bounds`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Expansion</strong> <code>double</code></summary>

Expands bounds by this amount to account for margin of error.

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Threshold</strong> <code>double</code></summary>

Minimum overlap amount for two points to be counted.

Default: `0.1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold Measure</strong> <code>EPCGExMeanMeasure</code></summary>

How to interpret the threshold value.

| Option       | Description                         |
| ------------ | ----------------------------------- |
| **Relative** | Percentage (0-1) of averaged radius |
| **Discrete** | Distance in world space             |

Default: `Relative`

⚡ PCG Overridable

</details>

#### Outputs

<details>

<summary><strong>Write Overlap Count</strong> <code>bool</code></summary>

Writes the number of different collections that overlap this point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Overlap Sub Count</strong> <code>bool</code></summary>

Writes the total number of overlapping points from other collections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Relative Overlap Count</strong> <code>bool</code></summary>

Writes overlap count relative to the maximum across all collections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Relative Overlap Sub Count</strong> <code>bool</code></summary>

Writes sub count relative to the maximum across all collections.

Default: `false`

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Any Overlap</strong> <code>bool</code></summary>

Adds a tag to collections where any point has overlap.

Default: `false`

</details>

<details>

<summary><strong>Tag If Has No Overlap</strong> <code>bool</code></summary>

Adds a tag to collections where no points have overlap.

Default: `false`

</details>

### Outputs

| Pin     | Type   | Description                    |
| ------- | ------ | ------------------------------ |
| **Out** | Points | Points with overlap statistics |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleOverlapStats.h)
