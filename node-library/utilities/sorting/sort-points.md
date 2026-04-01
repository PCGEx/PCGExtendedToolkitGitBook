---
description: Sort the source points according to specific rules.
icon: circle-plus
---

# Sort Points

### Overview

This node reorders points within each collection based on connected sorting rules. Each rule specifies an attribute to compare, and points are sorted by comparing these values in priority order. When two points have equal values for one rule, the next rule in priority order is used as a tiebreaker. This enables multi-level sorting for complex ordering requirements.

### How It Works

1. **Gather Rules**: Collects sorting rules from connected sub-nodes, ordered by priority.
2. **Compare Points**: For each pair of points, evaluates rules in order until a difference is found.
3. **Reorder Points**: Sorts points within each collection according to the comparison results.

**Usage Notes**

* **Multi-Level Sorting**: Connect multiple Sorting Rule sub-nodes for hierarchical sorting (e.g., sort by category, then by value within each category).
* **Priority Order**: Rules with lower priority values are evaluated first.
* **Tolerance**: Each rule has a tolerance for considering values equal, triggering tiebreaker rules.
* **Tag Values**: Rules can optionally read from data tags instead of point attributes.

### Behavior

**Multi-Level Sort:**

```
Input Points:
   Point A: Category = 2, Value = 10
   Point B: Category = 1, Value = 30
   Point C: Category = 1, Value = 20
   Point D: Category = 2, Value = 5

Rules (Ascending):
   1. Category (Priority 0)
   2. Value (Priority 1)

Output Order:
   Point B: Category = 1, Value = 30  ← Category 1, highest value
   Point C: Category = 1, Value = 20  ← Category 1, lower value
   Point D: Category = 2, Value = 5   ← Category 2, lowest value
   Point A: Category = 2, Value = 10  ← Category 2, higher value
```

### Inputs

| Pin           | Type   | Description                                              |
| ------------- | ------ | -------------------------------------------------------- |
| **In**        | Points | Input point collections to sort                          |
| **SortRules** | Params | Sorting Rule sub-nodes defining sort criteria (required) |

### Settings

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Controls the overall order in which points will be sorted.

| Option         | Description                     |
| -------------- | ------------------------------- |
| **Ascending**  | Lowest values first (A→Z, 0→9)  |
| **Descending** | Highest values first (Z→A, 9→0) |

Default: `Ascending`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                 |
| ------- | ------ | ------------------------------------------- |
| **Out** | Points | Points reordered according to sorting rules |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/Sorting/PCGExModularSortPoints.h)
