---
description: >-
  Tests whether points intersect or are contained within target bounding
  volumes.
icon: circle-dashed
---

# Filter : Inclusion (Bounds)

### Overview

This filter evaluates the spatial relationship between input points and a set of target bounds provided via an input pin. It supports various check types including intersection, containment, and boundary tests. Points can be tested against individual per-point bounds or collection-level bounds, using either box or sphere approximations.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Bounds Collection**: Gathers oriented bounding boxes from the connected bounds data.
2. **Point Evaluation**: For each input point, tests its position or bounds against all target bounds.
3. **Check Type**: Performs the configured spatial test (intersects, inside, etc.).
4. **Result Aggregation**: Returns pass if any target bound satisfies the check condition.

**Usage Notes**

* **Bounds Input**: Requires a "Bounds" input pin with point data defining the test volumes.
* **Self-Filtering**: Can optionally ignore self-intersection when testing against the same collection.
* **Expansion**: Use expansion to create tolerance zones around target bounds.

### Behavior

**Check Type Examples:**

```
Target Bounds: [Box at origin, 100x100x100]

Point A (inside):      Position (10, 10, 10)
Point B (on surface):  Position (50, 0, 0)
Point C (outside):     Position (200, 0, 0)
Point D (intersects):  Position (60, 0, 0) with 20-unit radius bounds

Intersects:           A=Pass, B=Pass, C=Fail, D=Pass
Is Inside:            A=Pass, B=Fail, C=Fail, D=Fail
Is Inside or On:      A=Pass, B=Pass, C=Fail, D=Fail
Is Inside or Intersects: A=Pass, B=Pass, C=Fail, D=Pass
```

### Inputs

| Pin        | Type   | Description                                               |
| ---------- | ------ | --------------------------------------------------------- |
| **Bounds** | Points | Target bounds data - each point defines a bounding volume |

### Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExBoundsFilterCompareMode</code></summary>

How to compare bounds.

| Option                | Description                                 |
| --------------------- | ------------------------------------------- |
| **Per Point Bounds**  | Test each input point individually          |
| **Collection Bounds** | Test using the collection's combined bounds |

Default: `Per Point Bounds`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bounds Target</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use from the target bounds data (connected to filter).

| Option               | Description                      |
| -------------------- | -------------------------------- |
| **Scaled Bounds**    | Use bounds scaled by point scale |
| **Density Bounds**   | Use bounds based on density      |
| **Steepness Bounds** | Use bounds based on steepness    |

Default: `Scaled Bounds`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Check Type</strong> <code>EPCGExBoundsCheckType</code></summary>

The type of spatial test to perform.

| Option                      | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| **Intersects**              | Point's OBB overlaps target OBBs                        |
| **Is Inside**               | Point center is inside target OBBs                      |
| **Is Inside or On**         | Point center is inside or on boundary of target OBBs    |
| **Is Inside or Intersects** | Point center inside OR point's OBB overlaps target OBBs |

Default: `Intersects`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use for the input points being filtered.

Default: `Scaled Bounds`

⚡ PCG Overridable

📋 _Visible when Check Type involves intersection tests_

</details>

<details>

<summary><strong>Test Mode</strong> <code>EPCGExBoxCheckMode</code></summary>

Shape approximation for testing.

| Option              | Description                  |
| ------------------- | ---------------------------- |
| **Box**             | Use oriented bounding box    |
| **Sphere**          | Use bounding sphere          |
| **Expanded Box**    | Use expanded bounding box    |
| **Expanded Sphere** | Use expanded bounding sphere |

Default: `Box`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Expansion</strong> <code>double</code></summary>

Epsilon value to expand target bounds, creating a tolerance zone.

Default: `10`

⚡ PCG Overridable

📋 _Visible when Test Mode is Expanded Box or Expanded Sphere_

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the result of the test.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Check Against Data Bounds</strong> <code>bool</code></summary>

Use collection bounds as a single proxy point instead of per-point testing.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy, Missing Data Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExBoundsFilter.h)
