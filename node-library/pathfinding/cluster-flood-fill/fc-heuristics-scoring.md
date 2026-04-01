---
description: Compute and accumulate heuristic scores for diffusion candidates.
icon: circle-dashed
---

# FC : Heuristics Scoring

### Overview

This fill control sub-node computes heuristic scores for diffusion candidates using connected heuristic sub-nodes. Unlike Heuristics Budget, this control only scores candidates without imposing stopping conditions. The scores influence candidate priority during expansion but don't block diffusion.

### How It Works

1. **Receive Heuristics**: Accepts heuristic sub-nodes via the Heuristics input pin.
2. **Score Candidates**: For each candidate, computes scores using all connected heuristics.
3. **Combine Scores**: Combines multiple heuristic scores using the selected scoring mode.
4. **Weight and Apply**: Applies the score weight and updates candidate priority.

**Usage Notes**

* **Scoring Only**: This control only scores, it never stops diffusion. Use Heuristics Budget or Heuristics Threshold if you need stopping conditions.
* **Priority Influence**: Higher scores give candidates higher priority during expansion.
* **Multiple Controls**: Can be combined with other scoring controls - scores are accumulated.

### Behavior

**Heuristics Scoring:**

```
Candidates scored by connected heuristics:
  [Vtx A] → Score: 0.8 (high priority)
  [Vtx B] → Score: 0.3 (low priority)
  [Vtx C] → Score: 0.6 (medium priority)

Expansion order influenced by scores:
  A (0.8) → C (0.6) → B (0.3)
```

**Scoring Components:**

```
  Local Score:    Score for this specific candidate
  Global Score:   Score relative to all candidates
  Previous Score: Inherited score from parent in path
```

### Inputs

| Pin            | Type   | Description                                        |
| -------------- | ------ | -------------------------------------------------- |
| **Heuristics** | Params | Heuristic sub-nodes for computing candidate scores |

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

<summary><strong>Scoring</strong> <code>EPCGExFloodFillHeuristicFlags</code></summary>

Bitmask controlling which score components are used.

| Flag               | Description                                      |
| ------------------ | ------------------------------------------------ |
| **Local Score**    | Score computed for this specific candidate       |
| **Global Score**   | Score relative to all candidates in the search   |
| **Previous Score** | Score inherited from the parent node in the path |

Default: `LocalScore`

</details>

<details>

<summary><strong>Score Weight</strong> <code>double</code></summary>

Multiplier applied to scores from this control. Higher values give this control more influence when combined with other scoring controls.

Default: `1.0`

Minimum: 0

⚡ PCG Overridable

</details>

#### Inherited Settings

This control does not use Source or Steps settings (scoring only, no validation).

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlHxScoring.h)
