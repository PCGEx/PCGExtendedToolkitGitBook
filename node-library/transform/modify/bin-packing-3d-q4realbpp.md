---
icon: circle
---

# Bin Packing 3D (Q4RealBPP)

{% hint style="info" %}
3D bin packing with real-world constraints from the [Q4RealBPP paper](https://www.nature.com/articles/s41598-023-39013-9): weight limits, category affinities, load bearing, and multi-objective scoring.
{% endhint %}

### Overview

This node packs items (points) into container bins using an advanced 3D bin packing algorithm adapted from the Q4RealBPP paper. It uses Extreme Point placement with multi-objective scoring that balances bin usage, height placement, load balance, and surface contact. Beyond basic geometric packing, it supports real-world constraints including weight limits per bin, physical load-bearing rules (items on top must be lighter), support requirements (items need a minimum base contact), and category-based affinity rules (which item types should or shouldn't share bins).

### How It Works

1. **Sorting**: Items are sorted by volume (largest-first for best-fit decreasing).
2. **Seed Setup**: Determines the starting position within each bin.
3. **Extreme Point Generation**: Maintains candidate placement positions (extreme points) within each bin.
4. **Placement Search**: For each item, evaluates all extreme points across all bins (if global), testing rotations and checking constraints (weight, support, load bearing, affinities).
5. **Multi-Objective Scoring**: Scores each valid placement using a weighted sum of bin usage, height, load balance, and contact objectives.
6. **Commit**: Places the item in the highest-scoring position, updates extreme points, and records the placement.
7. **Overflow**: Items that cannot be placed are output to the Discarded pin.

**Usage Notes**

* **Paper Reference**: Based on "Hybrid approach for solving real-world bin packing problem instances using quantum annealer" by Romero et al. (Nature Scientific Reports, 2023).
* **Extreme Points**: Unlike guillotine-cut free-space methods, Extreme Point placement tracks candidate positions on item corners, allowing more flexible packing.
* **Shorthand Selectors**: Many settings use shorthand selectors that can be either a constant value or read from an attribute. The default attribute name is shown in the selector.
* **Objective Weights**: The four objective weights do not need to sum to 1.0 -- they are relative weights that define the priority balance.

### Behavior

**Multi-Objective 3D Packing:**

```
Items sorted by volume, packed floor-up:

Bin:
┌──────────────────────────┐
│         ┌──────┐         │
│         │  C   │         │  ← C: lighter, placed on top
│  ┌──────┴──────┤         │
│  │      B      │  free   │  ← B: medium weight
│  ├─────────────┤         │
│  │      A      │         │  ← A: heaviest, placed first
│  └─────────────┘         │
└──────────────────────────┘

Scoring for placement of B:
  Bin Usage:     0.3 × (volume ratio)
  Height:        0.3 × (lower Z preferred)
  Load Balance:  0.2 × (closer to center preferred)
  Contact:       0.2 × (more surface contact preferred)
  → Weighted sum = final score
```

**Category Affinities:**

```
Rule: Category 0 ↔ Category 1 = Negative (Incompatible)

Item X (Category 0) → placed in Bin 1
Item Y (Category 1) → cannot go in Bin 1, placed in Bin 2
Item Z (Category 0) → can go in Bin 1

Rule: Category 2 ↔ Category 3 = Positive (Co-locate)

Item P (Category 2) → placed in Bin 3
Item Q (Category 3) → prefers Bin 3 (co-locate with P)
```

### Inputs

| Pin      | Type   | Description                                    |
| -------- | ------ | ---------------------------------------------- |
| **In**   | Points | Items to pack (uses point bounds as item size) |
| **Bins** | Points | Container bins (uses point bounds as bin size) |

### Settings

#### Sorting

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Controls the order in which items are processed. Descending processes the largest items first (best-fit decreasing).

| Option         | Description                 |
| -------------- | --------------------------- |
| **Ascending**  | Process smaller items first |
| **Descending** | Process larger items first  |

Default: `Descending`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort By Volume</strong> <code>bool</code></summary>

When enabled, items are sorted by bounding box volume before packing. This is the classic best-fit decreasing (BFD) approach.

Default: `true`

⚡ PCG Overridable

</details>

#### Seeding

<details>

<summary><strong>Seed Mode</strong> <code>EPCGExBinSeedMode</code></summary>

How the starting position for packing is determined within each bin.

| Option                 | Description                                        |
| ---------------------- | -------------------------------------------------- |
| **UVW Constant**       | Use a constant UVW position relative to bin bounds |
| **UVW Attribute**      | Read UVW position from a bin attribute             |
| **Position Constant**  | Use a constant world position                      |
| **Position Attribute** | Read world position from a bin attribute           |

Default: `UVW Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Seed UVW</strong> <code>FVector</code></summary>

The UVW position within the bin to start packing from. Values range from -1 to 1.

Default: `(-1, -1, -1)` (bottom-left-back corner)

⚡ PCG Overridable

📋 _Visible when Seed Mode = UVW Constant_

</details>

<details>

<summary><strong>Seed UVW Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the UVW seed position per bin.

⚡ PCG Overridable

📋 _Visible when Seed Mode = UVW Attribute_

</details>

<details>

<summary><strong>Seed Position</strong> <code>FVector</code></summary>

The world position to start packing from.

Default: `(0, 0, 0)`

⚡ PCG Overridable

📋 _Visible when Seed Mode = Position Constant_

</details>

<details>

<summary><strong>Seed Position Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the world seed position per bin.

⚡ PCG Overridable

📋 _Visible when Seed Mode = Position Attribute_

</details>

#### Packing

<details>

<summary><strong>Rotation Mode</strong> <code>EPCGExBP3DRotationMode</code></summary>

How many orientations to test when placing each item. More rotations produce better fits but are slower.

| Option                   | Description                                                                                                                               |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **None**                 | No rotation testing                                                                                                                       |
| **Cardinal (90deg)**     | Test 4 rotations around the up axis                                                                                                       |
| **Paper 6 Orientations** | 6 axis permutations with symmetry reduction from the Q4RealBPP paper. Cubes test 1, square prisms test 3, all-different dimensions test 6 |
| **All Orthogonal**       | Test all 24 orthogonal orientations                                                                                                       |

Default: `Paper 6 Orientations`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Global Best Fit</strong> <code>bool</code></summary>

When enabled, each item evaluates all available extreme points across all bins to find the globally best placement. When disabled, only searches within the current bin.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Occupation Padding</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Per-item padding added around each item's bounds. Can be a constant vector or read from a per-point attribute.

Default attribute: `Padding` · Default constant: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute Padding</strong> <code>bool</code></summary>

When enabled, padding is not rotated with the item. When disabled, padding rotates with item orientation.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Enable Load Bearing</strong> <code>bool</code></summary>

Enables the load-bearing constraint (Paper Eq. 26). Items placed on top of others must be lighter than the items supporting them, controlled by the threshold ratio.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Load Bearing Threshold</strong> <code>FPCGExInputShorthandSelectorDoubleAbs</code></summary>

Per-item mass ratio threshold (eta). The new item's weight must be less than or equal to eta times the supporting item's weight. A value of 1.0 means equal weight is allowed; 0.5 means the top item must be at most half the weight.

Default attribute: `LoadBearingThreshold` · Default constant: `1.0`

⚡ PCG Overridable

📋 _Visible when Enable Load Bearing is enabled_

</details>

<details>

<summary><strong>Require Support</strong> <code>bool</code></summary>

Requires items placed above the floor to have physical support beneath them, preventing visually floating items.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Support Ratio</strong> <code>FPCGExInputShorthandSelectorDouble01</code></summary>

Per-item minimum fraction of base area that must be supported. 0 means any contact is sufficient, 1 means the full base must be supported.

Default attribute: `MinSupportRatio` · Default constant: `0.2`

⚡ PCG Overridable

📋 _Visible when Require Support is enabled_

</details>

#### Objectives

<details>

<summary><strong>Bin Usage Weight</strong> <code>double</code></summary>

Weight for the bin-usage objective (Paper Eq. 1). Higher fill-ratio bins are preferred. Promotes efficient use of bin volume.

Default: `0.3`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Height Weight</strong> <code>double</code></summary>

Weight for the height objective (Paper Eq. 2). Lower placement positions are preferred, promoting floor-up packing.

Default: `0.3`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Load Balance Weight</strong> <code>double</code></summary>

Weight for the load-balance objective (Paper Eq. 3). Placements closer to the bin's center-of-mass are preferred, promoting even weight distribution.

Default: `0.2`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Contact Weight</strong> <code>double</code></summary>

Weight for the surface-contact objective. Items touching more surfaces (walls or other items) are preferred, promoting visual stability.

Default: `0.2`

⚡ PCG Overridable

</details>

#### Weight Constraint

<details>

<summary><strong>Enable Weight Constraint</strong> <code>bool</code></summary>

Enables a maximum weight per bin constraint (Paper Eq. 23). Items will not be placed in a bin if doing so would exceed the bin's weight limit.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Item Weight</strong> <code>FPCGExInputShorthandSelectorDoubleAbs</code></summary>

Per-item weight value.

Default attribute: `Weight` · Default constant: `1.0`

⚡ PCG Overridable

📋 _Visible when Enable Weight Constraint is enabled_

</details>

<details>

<summary><strong>Bin Max Weight</strong> <code>FPCGExInputShorthandSelectorDoubleAbs</code></summary>

Per-bin maximum weight capacity (read from bin points).

Default attribute: `MaxWeight` · Default constant: `100.0`

⚡ PCG Overridable

📋 _Visible when Enable Weight Constraint is enabled_

</details>

#### Category Affinities

<details>

<summary><strong>Enable Affinities</strong> <code>bool</code></summary>

Enables category-based affinity constraints (Paper Eq. 24-25). Items can be assigned integer categories, and rules define which categories should or shouldn't share bins.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Item Category</strong> <code>FPCGExInputShorthandSelectorInteger32</code></summary>

Per-item integer category value.

Default attribute: `Category` · Default constant: `-1`

⚡ PCG Overridable

📋 _Visible when Enable Affinities is enabled_

</details>

<details>

<summary><strong>Affinity Rules</strong> <code>TArray&#x3C;FPCGExBP3DAffinityRule></code></summary>

List of affinity rules between category pairs. Each rule specifies two categories and their relationship.

**FPCGExBP3DAffinityRule:**

| Property       | Type                     | Description                                                                                                              |
| -------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Type**       | `EPCGExBP3DAffinityType` | `Negative (Incompatible)` -- categories must NOT share a bin, or `Positive (Co-locate)` -- categories SHOULD share a bin |
| **Category A** | `int32`                  | First category value                                                                                                     |
| **Category B** | `int32`                  | Second category value                                                                                                    |

⚡ PCG Overridable

📋 _Visible when Enable Affinities is enabled_

</details>

#### Warnings

<details>

<summary><strong>Quiet Too Many Bins Warning</strong> <code>bool</code></summary>

When enabled, suppresses warnings when there are more bins than input items.

Default: `false`

</details>

<details>

<summary><strong>Quiet Too Few Bins Warning</strong> <code>bool</code></summary>

When enabled, suppresses warnings when there are fewer bins than input items.

Default: `false`

</details>

### Outputs

| Pin        | Type   | Description                                       |
| ---------- | ------ | ------------------------------------------------- |
| **Out**    | Points | Successfully packed items with updated transforms |
| **Bins**   | Points | The bin points (pass-through)                     |
| **Labels** | Points | Labeling/metadata output                          |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/Layout/PCGExBinPacking3D.h)
