---
description: >-
  Advanced box extraction with per-node weights, axis bias, and heuristic merge
  gating.
icon: function
---

# Decomp : Max Boxes (Extended)

### Overview

Extends the [decomp-max-boxes.md](decomp-max-boxes.md "mention") algorithm with additional scoring criteria. Per-node weights influence which regions are extracted first. Axis bias controls preferred box shapes (flat, tall, or cubic). Volume preference nudges extraction toward a target size range. Optional heuristic merge gating prevents merging cells across high-score cluster edges, preserving topological boundaries.

### How It Works

1. **Voxelization**: Same as  [decomp-max-boxes.md](decomp-max-boxes.md "mention"). The cluster is mapped onto a 3D occupancy grid.
2. **Weight & Bias Loading**: Per-node weight and axis bias values are read from attributes or constants. 3D prefix sums are built for O(1) region queries.
3. **Box Extraction**: Iteratively extracts the highest-scoring box. Scoring combines volume, compactness, average weight, axis bias, and volume preference.
4. **Priority Mode**: When Weight Mode is Priority, a two-pass extraction is used. Pass 1 only extracts boxes whose average weight exceeds the Priority Threshold. Pass 2 extracts remaining occupied space normally.
5. **Merge Gating**: If enabled, adjacent cells are only merged when the average heuristic score of boundary edges stays below the Merge Score Threshold. This prevents merging across topologically significant boundaries.

### Settings

#### Grid

<details>

<summary><strong>Transform Space</strong> <code>EPCGExDecompTransformSpace</code></summary>

How to orient the voxel grid.

| Option       | Description                                     |
| ------------ | ----------------------------------------------- |
| **World**    | Use world-space axes                            |
| **Best Fit** | Orient the grid to the cluster's principal axes |
| **Custom**   | Use a user-specified transform                  |

Default: `World`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom Transform</strong> <code>FTransform</code></summary>

Custom transform for grid alignment.

Default: `Identity`

📋 _Visible when Transform Space = Custom_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Voxel Size Mode</strong> <code>EPCGExDecompVoxelSizeMode</code></summary>

How to determine the voxel grid resolution.

| Option            | Description                      |
| ----------------- | -------------------------------- |
| **Edge Inferred** | Derive from cluster edge lengths |
| **Manual**        | User-specified                   |

Default: `Edge Inferred`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Voxel Size</strong> <code>FVector</code></summary>

Manual voxel size.

Default: `(100, 100, 100)`

📋 _Visible when Voxel Size Mode = Manual_

⚡ PCG Overridable

</details>

#### Box Extraction

<details>

<summary><strong>Max Cell Size</strong> <code>FVector</code></summary>

Maximum dimensions for output cells in world units. Larger boxes are subdivided.

Default: `(500, 500, 500)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Voxels Per Cell</strong> <code>int32</code></summary>

Minimum occupied voxels per cell. Cells below this are discarded.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Balance</strong> <code>double</code></summary>

Penalizes elongated strips. 0 = pure volume, higher values prefer compact shapes.

Default: `1.0`

⚡ PCG Overridable

</details>

#### Axis Bias

<details>

<summary><strong>Axis Bias</strong> <code>FVector (Constant/Attribute)</code></summary>

Per-axis compactness penalty. Use `(0.1, 0.1, 1)` for flat boxes or `(1, 1, 0.1)` for tall columns. Can be driven by a per-node vector attribute.

Default: `(1, 1, 1)`

⚡ PCG Overridable

</details>

#### Per-Node Weight

<details>

<summary><strong>Weight</strong> <code>double (Constant/Attribute)</code></summary>

Per-node weight for box scoring. Higher-weight regions are preferred during extraction.

Default: `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Influence</strong> <code>double</code></summary>

How strongly weights affect scoring. 0 = ignore weights, 1 = linear influence.

Default: `1.0`

📋 _Visible when Weight is attribute-driven_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Mode</strong> <code>EPCGExDecompWeightMode</code></summary>

| Option         | Description                                                       |
| -------------- | ----------------------------------------------------------------- |
| **Multiplier** | Weights scale box scores directly                                 |
| **Priority**   | Two-pass: high-weight boxes extracted first, then remaining space |

Default: `Multiplier`

📋 _Visible when Weight is attribute-driven_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Priority Threshold</strong> <code>double</code></summary>

Minimum average weight for Pass 1 extraction in Priority mode.

Default: `0.5`

📋 _Visible when Weight Mode = Priority_

⚡ PCG Overridable

</details>

#### Volume Preference

<details>

<summary><strong>Preferred Min Volume</strong> <code>double</code></summary>

Soft preference for minimum box volume in voxels. 0 = no minimum preference.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preferred Max Volume</strong> <code>double</code></summary>

Soft preference for maximum box volume in voxels. 0 = no maximum preference.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Volume Preference Weight</strong> <code>double</code></summary>

How strongly volume preference affects scoring.

Default: `1.0`

⚡ PCG Overridable

</details>

#### Heuristic Merge Gating

<details>

<summary><strong>Use Heuristic Merge Gating</strong> <code>bool</code></summary>

Enable heuristic-based merge control. Requires a Heuristics input on the parent Decomposition node.

Default: `false`

</details>

<details>

<summary><strong>Merge Score Threshold</strong> <code>double</code></summary>

Boundary edge score above which merging is blocked. Lower values are stricter (less merging). At 1.0, all merges are allowed.

Default: `0.5`

📋 _Visible when Use Heuristic Merge Gating = true_

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompMaxBoxesExt.h)
