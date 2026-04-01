---
description: Find the closest vtx or edge on each cluster' bounds.
icon: circle
---

# Cluster : Find point on Bounds

### Overview

This node identifies the vertex or edge point closest to a specified position on each cluster's bounding box. Given a UVW coordinate within the bounds, it finds which cluster element is nearest to that location, useful for identifying entry points, boundary vertices, or edge points at specific positions relative to the cluster's extent.

### How It Works

1. **Bounds Calculation**: Computes the bounding box for each cluster (optionally using best-fit orientation)
2. **Target Position**: Converts UVW coordinates to a world position on the bounds
3. **Proximity Search**: Finds the closest vertex or edge to the target position
4. **Output Generation**: Outputs the found points, either merged or per-cluster

**Usage Notes**

* **UVW Coordinates**: Use -1 to 1 range where -1 is min bound, 0 is center, 1 is max bound.
* **Best Fit Bounds**: Enable for non-axis-aligned clusters to get oriented bounding boxes.
* **Search Mode**: Choose whether to find closest vertices or edge midpoints.

### Inputs

| Pin       | Type   | Description           |
| --------- | ------ | --------------------- |
| **Vtx**   | Points | Cluster vertex points |
| **Edges** | Points | Cluster edge data     |

### Settings

#### Search

<details>

<summary><strong>Search Mode</strong> <code>EPCGExClusterClosestSearchMode</code></summary>

What type of cluster element to search for.

| Option           | Description                                    |
| ---------------- | ---------------------------------------------- |
| **Closest vtx**  | Find the vertex closest to the bounds position |
| **Closest edge** | Find the edge closest to the bounds position   |

Default: `Closest vtx`

</details>

#### Output

<details>

<summary><strong>Output Mode</strong> <code>EPCGExPointOnBoundsOutputMode</code></summary>

How to output the found points.

| Option                | Description                             |
| --------------------- | --------------------------------------- |
| **Merged Points**     | All found points in a single collection |
| **Per-point dataset** | Separate collection per cluster         |

Default: `Merged Points`

</details>

#### Bounds Configuration

<details>

<summary><strong>Best Fit Bounds</strong> <code>bool</code></summary>

Use oriented bounding box based on best-fit plane instead of axis-aligned bounds.

Default: `false`

</details>

<details>

<summary><strong>Axis Order</strong> <code>EPCGExAxisOrder</code></summary>

Priority order for axis alignment when computing best-fit bounds.

Default: `Y > X > Z`

_Visible when Best Fit Bounds = true_

</details>

#### Target Position

<details>

<summary><strong>UVW Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the UVW coordinate.

| Option        | Description                |
| ------------- | -------------------------- |
| **Constant**  | Use a fixed UVW value      |
| **Attribute** | Read UVW from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>UVW</strong> <code>FVector</code></summary>

Position within bounds as UVW coordinates. Range -1 to 1 where -1 is minimum, 0 is center, 1 is maximum.

Default: `(-1, -1, 0)` (bottom-left corner at mid-height)

_Visible when UVW Input = Constant_

</details>

<details>

<summary><strong>Element</strong> <code>EPCGExClusterElement</code></summary>

Which cluster element to read the UVW attribute from.

| Option    | Description           |
| --------- | --------------------- |
| **Point** | Read from vertex data |
| **Edge**  | Read from edge data   |

Default: `Edge`

_Visible when UVW Input = Attribute_

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Offset to apply to the found point, moving it away from the bounds center.

Default: `1`

</details>

#### Carry Over Settings

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are preserved from input to output.

//→ See TODO FPCGExCarryOverDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin     | Type   | Description                          |
| ------- | ------ | ------------------------------------ |
| **Out** | Points | Points at the found bounds positions |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExFindPointOnBoundsClusters.h)
