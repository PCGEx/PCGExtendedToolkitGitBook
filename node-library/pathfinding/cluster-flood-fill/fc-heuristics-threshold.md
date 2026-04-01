---
description: Stop diffusion when instantaneous heuristic crosses a threshold.
icon: circle-dashed
---

# FC : Heuristics Threshold

### Overview

This fill control sub-node computes heuristic scores and stops diffusion when a score crosses a threshold. Unlike Heuristics Budget which tracks accumulated cost over the path, this control evaluates single-step values (edge score, global score, or score delta) against the threshold at each expansion.

### How It Works

1. **Compute Heuristics**: Uses connected heuristic sub-nodes to score each candidate.
2. **Select Score Source**: Extracts the relevant score based on Threshold Source setting.
3. **Compare to Threshold**: Tests the score against the threshold using the comparison operator.
4. **Stop or Continue**: If comparison fails, diffusion stops in that direction.

**Usage Notes**

* **Instantaneous vs Accumulated**: Use this for per-step limits (e.g., "don't cross edges with score > 0.8"). Use Heuristics Budget for path-total limits.
* **Score Delta**: The Score Delta source measures the change from the previous candidate, useful for detecting sudden changes.
* **Dual Purpose**: This control both scores candidates and validates them.

### Behavior

```
Heuristics Threshold (Threshold: 0.5, Comparison: StrictlySmaller):

Edge scores along path:
  Seed → [0.2] → [0.4] → [0.3] → [0.7] → STOP
                                   ↑
                            0.7 >= 0.5, fails threshold

Threshold Sources:
  Edge Score:   Score for this specific edge/candidate
  Global Score: Total heuristic distance from seed
  Score Delta:  Change from previous step (|current - previous|)
```

### Inputs

| Pin            | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| **Heuristics** | Params | Heuristic sub-nodes for computing scores |

### Settings

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

How to combine scores when multiple heuristics are connected.

| Option               | Description                                               |
| -------------------- | --------------------------------------------------------- |
| **Weighted Average** | Average scores weighted by each heuristic's weight factor |
| **Min**              | Use the minimum score across all heuristics               |
| **Max**              | Use the maximum score across all heuristics               |
| **Sum**              | Sum all heuristic scores                                  |

Default: `WeightedAverage`

</details>

<details>

<summary><strong>Threshold Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the threshold comes from a constant or attribute.

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use a fixed threshold value      |
| **Attribute** | Read threshold from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Threshold (Attr)</strong> <code>FName</code></summary>

Attribute name to read the threshold from.

Default: `Threshold`

📋 _Visible when Threshold Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold</strong> <code>double</code></summary>

The threshold value to compare scores against.

Default: `0.5`

📋 _Visible when Threshold Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

Comparison operator. Candidate is valid if: `ThresholdSource [Comparison] Threshold`

| Option              | Description                                   |
| ------------------- | --------------------------------------------- |
| **StrictlyGreater** | Valid if score > threshold                    |
| **StrictlySmaller** | Valid if score < threshold                    |
| **GreaterOrEquals** | Valid if score >= threshold                   |
| **SmallerOrEquals** | Valid if score <= threshold                   |
| **NearlyEqual**     | Valid if score ≈ threshold (within tolerance) |
| **NearlyNotEqual**  | Valid if score ≉ threshold                    |

Default: `StrictlySmaller`

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance for near-equal comparisons.

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison is NearlyEqual or NearlyNotEqual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold Source</strong> <code>EPCGExFloodFillThresholdSource</code></summary>

Which score value to compare against the threshold.

| Option           | Description                                     |
| ---------------- | ----------------------------------------------- |
| **Edge Score**   | Current edge's heuristic score (single step)    |
| **Global Score** | Total heuristic distance from seed to candidate |
| **Score Delta**  | Change in score from the previous candidate     |

Default: `EdgeScore`

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source

Note: The Steps setting is not available for this control.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlHxThreshold.h)
