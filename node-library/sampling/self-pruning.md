---
description: A slower, more precise self pruning node.
icon: circle
---

# Self Pruning

### Overview

This node removes overlapping points within a single point collection based on their bounds. It offers more precision than simpler pruning methods by using oriented bounding box testing and configurable expansion values. Points can be sorted to control which overlapping points are kept, or the node can output overlap counts instead of pruning.

### How It Works

1. **Build Bounds**: Creates bounds for each point with optional expansion.
2. **Sort Points**: Orders points by configurable criteria for priority.
3. **Test Overlaps**: Detects bounds intersections using precise or fast tests.
4. **Prune or Write**: Either removes overlapping points or writes overlap counts.

**Usage Notes**

* **Prune Mode**: Removes overlapping points based on sort priority.
* **Write Result Mode**: Outputs overlap count instead of removing points.
* **Precise Test**: Uses oriented bounding boxes for more accurate overlap detection.
* **Expansion**: Grow bounds before or after transform to adjust overlap sensitivity.

### Behavior

**Self Pruning:**

```
Points with overlapping bounds:
   P0: bounds overlap P1, P2
   P1: bounds overlap P0
   P2: bounds overlap P0

Sort Direction = Ascending (lower index wins):
   → P0 kept (highest priority)
   → P1 pruned (overlaps P0)
   → P2 pruned (overlaps P0)

Write Result Mode:
   → P0.NumOverlaps = 2
   → P1.NumOverlaps = 1
   → P2.NumOverlaps = 1
```

### Inputs

| Pin               | Type   | Description                                   |
| ----------------- | ------ | --------------------------------------------- |
| **In**            | Points | Source points to prune                        |
| **Point Filters** | Params | Optional filters for which points can overlap |

### Settings

#### Mode

<details>

<summary><strong>Mode</strong> <code>EPCGExSelfPruningMode</code></summary>

How to handle overlapping points.

| Option           | Description                      |
| ---------------- | -------------------------------- |
| **Prune**        | Remove overlapping points        |
| **Write Result** | Write overlap count to attribute |

Default: `Prune`

</details>

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Order for determining which overlapping points have priority.

| Option         | Description        |
| -------------- | ------------------ |
| **Ascending**  | Lower indices win  |
| **Descending** | Higher indices win |

Default: `Ascending`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Randomize</strong> <code>bool</code></summary>

When enabled, adds random variation to sort order for more varied results.

Default: `true`

📋 _Visible when Mode = Prune_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Range</strong> <code>double</code></summary>

Random variation range (0-1) applied to sort values.

Default: `0.05`

📋 _Visible when Randomize is enabled_

⚡ PCG Overridable

</details>

#### Write Result Settings

<details>

<summary><strong>Output to</strong> <code>FName</code></summary>

Attribute name to write the overlap count.

Default: `NumOverlaps`

📋 _Visible when Mode = Write Result_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Units</strong> <code>EPCGExMeanMeasure</code></summary>

How to output the overlap count.

| Option       | Description                              |
| ------------ | ---------------------------------------- |
| **Discrete** | Raw overlap count                        |
| **Relative** | Normalized against highest overlap count |

Default: `Discrete`

📋 _Visible when Mode = Write Result_

⚡ PCG Overridable

</details>

<details>

<summary><strong>OneMinus</strong> <code>bool</code></summary>

When enabled, outputs (1 - normalized overlap) instead of normalized overlap.

Default: `false`

📋 _Visible when Mode = Write Result and Units = Relative_

⚡ PCG Overridable

</details>

#### Expansion

**This doesn't modify your points**

The expansion settings here are **only for overlap testing** — they don't actually change your point bounds. The node tests overlaps "as if" the bounds were expanded, then prunes based on those tests. Your output points keep their original `$BoundsMin`/`$BoundsMax` values.

If you need to actually modify point bounds, use a **Bounds Modifier** node before or after.

{% hint style="info" %}
### Why "Before/After Transform"?

Point bounds (`$BoundsMin`/`$BoundsMax`) are stored in **local space** — they don't include the point's scale, rotation, or position. To test overlaps in world space, the node must apply the point's transform.

The question is: do you want to expand **before** or **after** that transform is applied?

**Before Transform** — Works like PCG's Bounds Modifier. Expansion happens in local space, then gets scaled/rotated with the point.

* Expand by 10 on a point with scale 10 → effective expansion of 100 in world space
* Good when you want expansion proportional to point size

**After Transform** — Expansion happens in world space after all transforms.

* Expand by 10 → always exactly 10 units in world space, regardless of point scale
* Good when you want consistent spacing in world units

**Quick rule**: If you want behavior like Bounds Modifier, use **Before Transform**. If you want exact world-space distances, use **After Transform**.
{% endhint %}

{% hint style="info" %}
### Primary vs Secondary Bounds

Overlap testing checks each point against all others. These settings let you expand bounds differently for each role:

* **Primary bounds**: The point currently being evaluated ("Am I overlapping anything?")
* **Secondary bounds**: All other points being tested against ("What am I checking against?")

Usually you only need to expand one or the other. Expanding primary makes each point "claim more space." Expanding secondary makes neighbors appear larger.
{% endhint %}

<details>

<summary><strong>Precise Test</strong> <code>bool</code></summary>

When enabled, uses oriented bounding box (OBB) testing for more accurate overlap detection. Without this, tests use axis-aligned bounding boxes which can report false overlaps for rotated points.

Default: `false`

</details>

<details>

<summary><strong>Primary Mode</strong> <code>EPCGExSelfPruningExpandOrder</code></summary>

If and how to expand the primary bounds (the point being evaluated).

| Option               | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| **None**             | Do not expand bounds                                                        |
| **Before Transform** | Expand in local space before world transform (expansion rotates with point) |
| **After Transform**  | Expand in world space after transform (axis-aligned expansion)              |

Default: `None`

</details>

<details>

<summary><strong>Primary Expansion Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the primary expansion value.

| Option        | Description               |
| ------------- | ------------------------- |
| **Constant**  | Use fixed expansion value |
| **Attribute** | Read from attribute       |

Default: `Constant`

📋 _Visible when Primary Mode is not None_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Primary Expansion</strong> <code>double</code></summary>

Uniform offset applied to primary bounds. Positive values grow the bounds outward, making the point "claim more space" and overlap with more neighbors.

Default: `0`

📋 _Visible when Primary Mode is not None_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary Mode</strong> <code>EPCGExSelfPruningExpandOrder</code></summary>

If and how to expand the secondary bounds (neighbor points being tested against).

| Option               | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| **None**             | Do not expand bounds                                                        |
| **Before Transform** | Expand in local space before world transform (expansion rotates with point) |
| **After Transform**  | Expand in world space after transform (axis-aligned expansion)              |

Default: `None`

</details>

<details>

<summary><strong>Secondary Expansion Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the secondary expansion value.

| Option        | Description               |
| ------------- | ------------------------- |
| **Constant**  | Use fixed expansion value |
| **Attribute** | Read from attribute       |

Default: `Constant`

📋 _Visible when Secondary Mode is not None_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary Expansion</strong> <code>double</code></summary>

Uniform offset applied to secondary (neighbor) bounds. Expanding secondary bounds makes neighbors appear larger, increasing the chance the primary point overlaps them.

Default: `0`

📋 _Visible when Secondary Mode is not None_

⚡ PCG Overridable

</details>

### Outputs

| Pin     | Type   | Description                               |
| ------- | ------ | ----------------------------------------- |
| **Out** | Points | Pruned points or points with overlap data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSelfPruning.h)
