---
description: Stop diffusion when accumulated heuristic cost exceeds a budget.
icon: circle-dashed
---

# FC : Heuristics Budget

### Overview

This fill control sub-node combines heuristic scoring with budget-based stopping. It computes heuristic scores for each candidate using connected heuristic sub-nodes, accumulates these costs along the path, and stops diffusion when the total exceeds a maximum budget. This enables cost-aware expansion that considers multiple factors.

### How It Works

1. **Compute Scores**: Uses connected heuristic sub-nodes to score each candidate.
2. **Accumulate Cost**: Adds the candidate's score to the running total for that path.
3. **Check Budget**: Compares accumulated cost against the maximum budget.
4. **Stop When Exceeded**: When the budget is exceeded, diffusion stops in that direction.

**Usage Notes**

* **Dual Purpose**: This control both scores candidates (influencing priority) and validates them (stopping when over budget).
* **Budget Sources**: Can track path score, composite score, or spatial distance for budget comparison.
* **Heuristic Input**: Requires heuristic sub-nodes connected via the Heuristics pin to compute costs.

### Behavior

```
Heuristics Budget (MaxBudget = 50):

Path with heuristic costs:
  Seed → [10] → [15] → [20] → [10] → STOP

  Accumulated: 0    10    25    45    55 (exceeds 50)

Budget Sources:
  Path Score:      Accumulated heuristic score along path
  Composite Score: Total combined score from all heuristics
  Path Distance:   Accumulated spatial (edge length) distance
```

### Inputs

| Pin            | Type   | Description                                       |
| -------------- | ------ | ------------------------------------------------- |
| **Heuristics** | Params | Heuristic sub-nodes for computing candidate costs |

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

<summary><strong>Max Budget Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the maximum budget comes from a constant or attribute.

| Option        | Description                   |
| ------------- | ----------------------------- |
| **Constant**  | Use a fixed budget value      |
| **Attribute** | Read budget from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Max Budget (Attr)</strong> <code>FName</code></summary>

Attribute name to read the budget limit from.

Default: `MaxBudget`

📋 _Visible when Max Budget Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Budget</strong> <code>double</code></summary>

Maximum accumulated cost before diffusion stops.

Default: `100.0`

Minimum: 0

📋 _Visible when Max Budget Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Budget Source</strong> <code>EPCGExFloodFillBudgetSource</code></summary>

Which accumulated value to compare against the budget.

| Option              | Description                                 |
| ------------------- | ------------------------------------------- |
| **Path Score**      | Accumulated heuristic score along the path  |
| **Composite Score** | Total combined score from all heuristics    |
| **Path Distance**   | Accumulated spatial distance (edge lengths) |

Default: `PathScore`

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source

Note: The Steps setting is not available for this control.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlHxBudget.h)
