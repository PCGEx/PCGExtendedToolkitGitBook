---
description: >-
  Compares the distance between the tested point and another point inside the
  same dataset.
icon: circle-dashed
---

# Filter : Segment Length

### Overview

This filter measures the distance between each point and another point within the same collection, then compares that distance against a threshold. The target point can be specified as an absolute index or as a relative offset from the current point. This enables filtering based on spacing between neighbors, detecting gaps, or enforcing minimum/maximum segment lengths in ordered point sets.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Index Resolution**: Computes the target point index using Pick (absolute) or Offset (relative) mode.
2. **Distance Measurement**: Calculates the distance between the current point and the target point.
3. **Threshold Comparison**: Compares the measured distance against the threshold using the selected comparison operator.
4. **Result**: Returns pass if the comparison succeeds, or the fallback value for invalid indices.

**Usage Notes**

* **Offset Mode**: An offset of 1 compares each point against the next point, measuring the segment between them.
* **Squared Distance**: Enable this for faster comparisons when you only need relative ordering, since it skips the square root calculation. Remember to square your threshold value accordingly.
* **Closed Loops**: When "Tile on closed loops" is enabled and the data represents a closed loop, out-of-range indices wrap around rather than using the general safety mode.

### Behavior

**Segment Length (Offset Mode):**

```
Points: [P0, P1, P2, P3]
Index Offset: 1 (next point)
Threshold: 50
Comparison: >

P0→P1: dist=30  → 30 > 50: Fail (short segment)
P1→P2: dist=80  → 80 > 50: Pass (long segment)
P2→P3: dist=45  → 45 > 50: Fail (short segment)
P3→??: invalid  → Fallback: Fail
```

**Pick Mode:**

```
Points: [P0, P1, P2, P3]
Index Pick: 0 (first point)
Threshold: 100
Comparison: <=

Compare every point's distance to the first point against the threshold.
```

### Settings

<details>

<summary><strong>Threshold Input</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant threshold or read it from an attribute.

| Option        | Description                               |
| ------------- | ----------------------------------------- |
| **Constant**  | Use the specified constant threshold      |
| **Attribute** | Read the threshold from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read the threshold from.

⚡ PCG Overridable

📋 _Visible when Threshold Input = Attribute_

</details>

<details>

<summary><strong>Threshold</strong> <code>double</code></summary>

Constant threshold distance for comparison.

Default: `100`

⚡ PCG Overridable

📋 _Visible when Threshold Input = Constant_

</details>

<details>

<summary><strong>Squared Distance</strong> <code>bool</code></summary>

If enabled, compares against the squared distance instead of the actual distance. This is faster since it skips the square root calculation, but the threshold must be squared accordingly.

Default: `false`

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator to use between the measured distance and the threshold.

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

Default: `Strictly Greater`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Rounding mode for approximate comparison modes.

Default: `0`

⚡ PCG Overridable

📋 _Visible when Comparison is Nearly Equal or Nearly Not Equal_

</details>

<details>

<summary><strong>Index Mode</strong> <code>EPCGExIndexMode</code></summary>

How to interpret the index value.

| Option     | Description                                           |
| ---------- | ----------------------------------------------------- |
| **Pick**   | Use as an absolute index into the collection          |
| **Offset** | Use as a relative offset from the current point index |

Default: `Offset`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant index or read from an attribute.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Use the specified constant index      |
| **Attribute** | Read the index from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read the index from. Value is converted to int32.

⚡ PCG Overridable

📋 _Visible when Compare Against = Attribute_

</details>

<details>

<summary><strong>Index</strong> <code>int32</code></summary>

The constant index value. In Offset mode, 1 means next point, -1 means previous point.

Default: `1`

⚡ PCG Overridable

📋 _Visible when Compare Against = Constant_

</details>

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle out-of-range indices.

| Option     | Description                       |
| ---------- | --------------------------------- |
| **Ignore** | Skip the test for invalid indices |
| **Tile**   | Wrap around using modulo          |
| **Clamp**  | Clamp to valid range              |
| **Yoyo**   | Bounce back and forth             |

Default: `Clamp`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tile on closed loops</strong> <code>bool</code></summary>

If enabled, forces Tile index safety on closed loop paths regardless of the Index Safety setting.

Default: `true`

</details>

<details>

<summary><strong>Invalid Point Fallback</strong> <code>EPCGExFilterFallback</code></summary>

What the filter should return when the point required for computing length is invalid (e.g., first or last point with no valid neighbor).

| Option   | Description                    |
| -------- | ------------------------------ |
| **Pass** | Return pass for invalid points |
| **Fail** | Return fail for invalid points |

Default: `Fail`

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the result of the filter. Note that this also inverts fallback results.

Default: `false`

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExSegmentLengthFilter.h)
