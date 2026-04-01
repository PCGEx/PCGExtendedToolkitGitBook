---
description: Sort collection using @Data domain attributes.
icon: circle-plus
---

# Sort Data

### Overview

This node reorders entire collections within the output based on their @Data domain attribute values. Unlike Sort Points which reorders points within collections, this node changes the order of collections themselves. Connected sorting rules determine which @Data attributes to compare and in what priority order.

### How It Works

1. **Gather Rules**: Collects sorting rules from connected sub-nodes, ordered by priority.
2. **Read @Data**: For each collection, reads the specified @Data domain attribute values.
3. **Compare Collections**: Evaluates rules in order to determine collection ordering.
4. **Reorder Output**: Outputs collections in the sorted order.

**Usage Notes**

* **@Data Domain**: This node compares collection-level metadata, not individual point attributes.
* **Same Rules**: Uses the same Sorting Rule sub-nodes as Sort Points.
* **Stable Sort**: Collections with equal values maintain their relative input order.

### Behavior

```
Collection Sorting:

Input Collections:
   Collection A: @Data.Priority = 3
   Collection B: @Data.Priority = 1
   Collection C: @Data.Priority = 2

Direction = Ascending:
   Output: B, C, A (1, 2, 3)

Direction = Descending:
   Output: A, C, B (3, 2, 1)
```

### Inputs

| Pin           | Type   | Description                                   |
| ------------- | ------ | --------------------------------------------- |
| **In**        | Any    | Input data collections to sort                |
| **SortRules** | Params | Sorting Rule sub-nodes defining sort criteria |

### Settings

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Controls the order in which collections will be sorted.

| Option         | Description          |
| -------------- | -------------------- |
| **Ascending**  | Lowest values first  |
| **Descending** | Highest values first |

Default: `Ascending`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type | Description                                      |
| ------- | ---- | ------------------------------------------------ |
| **Out** | Any  | Collections reordered according to sorting rules |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/Sorting/PCGExSortCollections.h)
