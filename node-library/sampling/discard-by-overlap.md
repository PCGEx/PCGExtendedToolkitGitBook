---
description: Discard entire datasets based on how they overlap with each other.
icon: circle
---

# Discard By Overlap

> This is basically self-pruning for collections.

### Overview

Analyzes how multiple point collections overlap with each other and **discards entire datasets** based on weighted scoring. This is self-pruning for collections — it decides which overlapping datasets to keep and which to throw away.

### How It Works

1. **Compute bounds**: Build bounds for every point in every dataset.
2. **Detect overlaps**: Test all dataset pairs for overlapping regions, then drill into per-point overlap detection.
3. **Score**: Compute a weighted score for each dataset from overlap metrics and intrinsic properties.
4. **Prune**: Remove the worst-scoring dataset, update all remaining scores, repeat until no overlaps remain.

{% hint style="warning" %}
**Pruning is iterative.** When a dataset is pruned, its overlaps with other datasets are removed. This changes everyone else's dynamic scores, so the node re-scores and re-sorts after every prune. A dataset that looked like a bad candidate might become a good one after its main overlap partner is removed.
{% endhint %}

**Usage Notes**

* **Dataset-level**: Operates on entire collections, not individual points.
* **Only ratios matter**: Weights are internally normalized within each category, so `OverlapCount = 4, OverlapSubCount = 2` is identical to `OverlapCount = 2, OverlapSubCount = 1`.
* **Custom scoring**: Tag scores and `@Data` attribute scores let you bias the system toward keeping or discarding specific datasets regardless of their overlap.

### How Scoring Works

Each dataset gets a single **Weight** value that determines its pruning priority. This weight is a blend of two categories:

**Dynamic scores** — computed from actual overlaps, change as datasets are pruned:

| Metric                     | What it measures                                             |
| -------------------------- | ------------------------------------------------------------ |
| **Overlap Count**          | How many other datasets this one overlaps with               |
| **Overlap Sub Count**      | Total number of individual point-to-point overlaps           |
| **Overlap Volume**         | Cumulative intersection volume across all overlapping points |
| **Overlap Volume Density** | Overlap volume divided by overlapping point count            |

**Static scores** — intrinsic to the dataset, don't change during pruning:

| Metric               | What it measures                      |
| -------------------- | ------------------------------------- |
| **Num Points**       | How many points the dataset contains  |
| **Volume**           | Total volume of all point bounds      |
| **Volume Density**   | Points per unit of volume             |
| **Custom Tag Score** | Sum of score values for matching tags |
| **Data Score**       | Sum of `@Data` attribute values       |

Each raw score is **normalized relative to the current maximum** across all remaining datasets (so the highest becomes 1.0). The final weight is:

```
Weight = (StaticWeight × StaticBalance) + (DynamicWeight × DynamicBalance)
```

**Dynamic Balance** and **Static Balance** control how much each category matters. The default (`Dynamic = 1`, `Static = 0.5`) means overlap data has roughly twice the influence of intrinsic properties.

### Behavior

```
Input:  A (50pts, overlaps B+C), B (30pts, overlaps A), C (100pts, overlaps A)

Round 1 — Score all:
   A: overlaps 2 datasets → highest dynamic score → pruned
Round 2 — Re-score:
   B and C no longer overlap → both output
```

### Inputs

| Pin               | Type   | Description                                                        |
| ----------------- | ------ | ------------------------------------------------------------------ |
| **In**            | Points | Multiple point collections to analyze                              |
| **Point Filters** | Params | Optional filters for which points participate in overlap detection |

### Settings

#### Overlap Detection

<details>

<summary><strong>Test Mode</strong> <code>EPCGExOverlapTestMode</code></summary>

How to test for overlaps between points.

| Option     | Description                                                           |
| ---------- | --------------------------------------------------------------------- |
| **Fast**   | Only test using datasets' overall bounds                              |
| **Box**    | Test every point's bounds as transformed box (may miss some overlaps) |
| **Sphere** | Test every point's bounds as spheres (may have false positives)       |

Default: `Sphere`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use for overlap testing.

| Option             | Description                           |
| ------------------ | ------------------------------------- |
| **Scaled Bounds**  | Uses point bounds multiplied by scale |
| **Density Bounds** | Uses density-based bounds             |
| **Bounds**         | Uses raw point bounds                 |
| **Center**         | Uses point center only                |

Default: `Scaled Bounds`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Expansion</strong> <code>double</code></summary>

Expands bounds by this amount to account for margin of error.

Default: `10`

⚡ PCG Overridable

</details>

#### Scoring Weights

<details>

<summary><strong>Weighting</strong> <code>FPCGExOverlapScoresWeighting</code></summary>

See [How Scoring Works](discard-by-overlap.md#how-scoring-works) above for a full explanation of how these combine.

**Dynamic Weights** (from overlaps — change as datasets are pruned):

| Property                   | Default | Description                             |
| -------------------------- | ------- | --------------------------------------- |
| **Dynamic Balance**        | `1`     | Weight of dynamic category vs static    |
| **Overlap Count**          | `2`     | Weight for number of overlapping sets   |
| **Overlap Sub Count**      | `1`     | Weight for number of overlapping points |
| **Overlap Volume**         | `0`     | Weight for cumulative overlap volume    |
| **Overlap Volume Density** | `0`     | Weight for volume per overlapping point |

**Static Weights** (intrinsic to dataset — constant during pruning):

| Property              | Default | Description                            |
| --------------------- | ------- | -------------------------------------- |
| **Static Balance**    | `0.5`   | Weight of static category vs dynamic   |
| **Num Points**        | `1`     | Weight for point count                 |
| **Volume**            | `0`     | Weight for total dataset volume        |
| **Volume Density**    | `0`     | Weight for points per volume           |
| **Custom Tag Weight** | `0`     | Weight for tag-based scores            |
| **Tag Scores**        |         | Map of tags to score values            |
| **Data Score Weight** | `0`     | Weight for `@Data` attribute scores    |
| **Data Scores**       |         | List of `@Data` attribute names to sum |

⚡ PCG Overridable

</details>

#### Pruning Logic

<details>

<summary><strong>Logic</strong> <code>EPCGExOverlapPruningLogic</code></summary>

Order in which datasets are pruned.

| Option          | Description                    |
| --------------- | ------------------------------ |
| **Low to High** | Lower scores are pruned first  |
| **High to Low** | Higher scores are pruned first |

Default: `High to Low`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Threshold</strong> <code>double</code></summary>

Minimum overlap amount for two sub-points to be counted. Higher values require more overlap.

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

<details>

<summary><strong>Include Filtered in Metrics</strong> <code>bool</code></summary>

When enabled, filtered-out points still contribute to bounds and metrics calculations.

Default: `true`

⚡ PCG Overridable

</details>

### Outputs

| Pin     | Type   | Description                                |
| ------- | ------ | ------------------------------------------ |
| **Out** | Points | Datasets that survived the pruning process |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExDiscardByOverlap.h)
