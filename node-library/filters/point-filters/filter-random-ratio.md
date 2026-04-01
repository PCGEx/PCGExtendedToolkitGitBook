---
description: Filter using a random ratio-based selection.
icon: circle-dashed
---

# Filter : Random (Ratio)

### Overview

This filter selects an exact number or ratio of points from a collection using random selection. Unlike the threshold-based Random filter which gives each point an independent probability, this filter guarantees a specific count or percentage of points will be selected. It pre-computes the selection set so the exact number of passing points is deterministic.

### How It Works

1. **Count Resolution**: Determines how many points to select based on the ratio and units.
2. **Random Selection**: Randomly picks the determined number of indices using the seed.
3. **Set Evaluation**: For each point, checks if its index is in the pre-computed selection set.
4. **Result**: Returns pass if the point's index was selected, fail otherwise.

**Usage Notes**

* **Exact Count**: In Discrete mode, selects exactly the specified number of points. In Relative mode, selects a percentage of the total.
* **Clamping**: Optional min/max clamps control the final selection count.
* **Deterministic**: Same seed and input produce the same selection.

### Behavior

**Ratio Selection:**

```
Collection: 100 points
Units: Relative
Ratio: 0.3

Result: Exactly 30 randomly selected points pass
Remaining 70 points fail
```

**Discrete Selection:**

```
Collection: 100 points
Units: Discrete
Ratio: 10

Result: Exactly 10 randomly selected points pass
```

### Settings

<details>

<summary><strong>Random</strong> <code>FPCGExRandomRatioDetails</code></summary>

Random selection configuration.

**Base Seed** `FPCGExInputShorthandSelectorInteger32` The seed for random number generation. Can be read from an attribute or use a constant.

Default: `42` (from `@Data.Seed`)

**Units** `EPCGExMeanMeasure`

| Option       | Description                               |
| ------------ | ----------------------------------------- |
| **Relative** | Ratio is a fraction of total points (0-1) |
| **Discrete** | Ratio is an absolute count                |

Default: `Relative`

**Clamp Min** `bool` + `int32` When enabled, ensures at least this many points are selected.

**Clamp Max** `bool` + `int32` When enabled, ensures no more than this many points are selected.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Result</strong> <code>bool</code></summary>

Inverts the filter result.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExRandomRatioFilter.h)
