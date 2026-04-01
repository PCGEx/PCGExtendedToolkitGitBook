---
description: Routes point collections to multiple output pins based on filter conditions.
icon: circle-plus
---

# Uber Branch

### Overview

This control flow node evaluates point collections against multiple sets of filter conditions and routes each collection to matching output pins. Unlike single-condition branching, it supports multiple independent branches, each with their own filter inputs and output destinations. Collections are tested against each branch's filters in sequence, and routed to the first matching branch or a default output.

### How It Works

1. **Branch Generation**: Creates multiple input/output pin pairs based on NumBranches setting
2. **Filter Loading**: Loads filter factories from each branch's filter input pin
3. **Collection Testing**: Tests each input collection against branch filters in order
4. **First-Match Routing**: Routes collection to the first branch whose filters pass
5. **Default Fallback**: Routes collections that don't match any filter to the default output
6. **Parallel Processing**: Processes collections in parallel batches for efficiency

### Behavior

```
Configuration:

NumBranches: 3
Branches: 0, 1, 2 (plus Default)

Input Pins:
- In (main data)
- → 0 (filters for branch 0)
- → 1 (filters for branch 1)
- → 2 (filters for branch 2)

Output Pins:
- 0 → (branch 0 output)
- 1 → (branch 1 output)
- 2 → (branch 2 output)
- Default (fallback output)
```

**Routing Example:**

```
Branch 0 Filters: Density >= 0.8
Branch 1 Filters: Density >= 0.4
Branch 2 Filters: Density < 0.4

Collection A (Density = 0.9):
  Test Branch 0: PASS → Route to "0 →" ✓
  (Skip remaining branches)

Collection B (Density = 0.6):
  Test Branch 0: FAIL
  Test Branch 1: PASS → Route to "1 →" ✓
  (Skip remaining branches)

Collection C (Density = 0.2):
  Test Branch 0: FAIL
  Test Branch 1: FAIL
  Test Branch 2: PASS → Route to "2 →" ✓

Collection D (No Density):
  Test Branch 0: FAIL
  Test Branch 1: FAIL
  Test Branch 2: FAIL
  → Route to "Default" ✓
```

**Multiple Filters Per Branch:**

```
Branch 0 Filters:
- Attribute Filter (Type == "Tree")
- Bounds Filter (BoundsMax.Z > 500)

Collection passes Branch 0 only if ALL filters pass
(filter logic depends on filter manager mode)
```

**AsyncChunkSize:**

```
Collections: 100 total
AsyncChunkSize: 32

Processing:
- Batch 1: Collections 0-31 (parallel)
- Batch 2: Collections 32-63 (parallel)
- Batch 3: Collections 64-95 (parallel)
- Batch 4: Collections 96-99 (parallel)

AsyncChunkSize: 0
- All 100 collections processed in single batch
```

Good for: multi-tier classification, priority routing, category-based pipelines, conditional data streams, hierarchical filtering

### Settings

<details>

<summary><strong>Num Branches</strong> <code>int32</code></summary>

Number of branch filter groups to evaluate.

Each branch creates:

* One filter input pin (→ N)
* One data output pin (N →)

Examples:

* `3` (default): 3 branches (0, 1, 2) plus Default
* `5`: 5 branches (0, 1, 2, 3, 4) plus Default
* `1`: Single branch test plus Default

Branches are tested in order (0, 1, 2, ...). The first matching branch receives the collection.

Default: `3`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Async Chunk Size</strong> <code>int32</code></summary>

Number of collections to process in parallel batches.

Controls parallel processing granularity:

* **> 0**: Process N collections at a time in parallel
* **0**: Process all collections in a single batch

Examples:

* `32` (default): Process 32 collections per batch
* `64`: Larger batches (fewer parallel tasks)
* `0`: Single-batch processing (can be faster for simple filters)

Larger values reduce parallel overhead but may increase memory usage.

Default: `32`

</details>

### Filter Behavior

Collections are tested against branch filters using the filter manager system:

* Filters are combined based on their factory settings (AND, OR, NOT logic)
* A collection passes a branch if it satisfies the filter combination
* Collections route to the **first** matching branch (order matters)
* Collections that pass no filters route to Default output

### Inputs

| Pin     | Type             | Description                                  |
| ------- | ---------------- | -------------------------------------------- |
| **In**  | Point Data       | Point collections to route                   |
| **→ 0** | Filter Factories | Filters for branch 0                         |
| **→ 1** | Filter Factories | Filters for branch 1                         |
| **→ N** | Filter Factories | Filters for branch N (based on Num Branches) |

_Filter input pins are dynamically created based on Num Branches_

### Outputs

| Pin         | Type       | Description                            |
| ----------- | ---------- | -------------------------------------- |
| **0 →**     | Point Data | Collections matching branch 0 filters  |
| **1 →**     | Point Data | Collections matching branch 1 filters  |
| **N →**     | Point Data | Collections matching branch N filters  |
| **Default** | Point Data | Collections matching no branch filters |

_Output pins are dynamically created based on Num Branches_

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/ControlFlow/PCGExUberBranch.h)
