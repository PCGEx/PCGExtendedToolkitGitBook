---
description: >-
  Filter points into multiple buckets based on ordered filter groups. First
  matching group claims the point.
icon: circle-plus
---

# Uber Filter (Cascade)

### Overview

This node evaluates points against a series of filter groups in order, routing each point to the first group that matches. It works like a cascade of if/else-if conditions: each branch has its own filter input, and points are tested against branches sequentially. The first branch whose filters pass claims the point. Points that don't match any branch are optionally sent to a discard output.

### How It Works

1. **Branch Setup**: Creates N branches based on `NumBranches`, each with its own filter input pin and output pin
2. **Per-Point Evaluation**: For each point in the input, tests the point against Branch 0's filters first
3. **First Match Wins**: If filters pass, the point is routed to Branch 0's output and evaluation stops for that point. If filters fail, the point is tested against Branch 1, then Branch 2, and so on
4. **Discard Handling**: Points that don't match any branch are sent to the Outside output (if enabled)

**Usage Notes**

* **Priority Ordering**: Branch order determines priority. Place the most specific or important filter group first, broader catches later. This is a waterfall — once a point is claimed, it's gone from subsequent branches.
* **Dynamic Pin Count**: Changing `NumBranches` adds or removes paired input/output pins. Each branch gets a filter input (`→ 0`, `→ 1`, ...) and a corresponding output (`0 →`, `1 →`, ...).
* **Empty Branches**: A branch with no filters connected will match nothing — points pass through to the next branch.
* **Point Filters**: Each branch accepts standard point filter sub-nodes. Multiple filters on the same branch combine using their normal composition logic.

### Behavior

```
Input Points: [A, B, C, D, E, F]

Branch 0 (filters: "attribute > 10"):
  A passes → routed to Output 0
  D passes → routed to Output 0

Branch 1 (filters: "tag = 'special'"):
  B passes → routed to Output 1
  (A already claimed — not tested)

Branch 2 (filters: "bounds overlap"):
  C passes → routed to Output 2
  E passes → routed to Output 2

Remaining: [F]
  → Routed to Outside (if enabled)
```

```
Output 0 →  [A, D]
Output 1 →  [B]
Output 2 →  [C, E]
Outside  →  [F]
```

### Inputs

| Pin     | Type             | Description                                               |
| ------- | ---------------- | --------------------------------------------------------- |
| **In**  | Point Data       | Points to filter into branches                            |
| **→ 0** | Filter Factories | Filter group for Branch 0                                 |
| **→ 1** | Filter Factories | Filter group for Branch 1                                 |
| **→ N** | Filter Factories | Filter group for Branch N (dynamic, based on NumBranches) |

### Settings

<details>

<summary><strong>Num Branches</strong> <code>int32</code></summary>

Number of filter groups (branches) to evaluate. Each branch gets a paired input/output pin. Increasing this adds more branches; decreasing removes them from the end.

Default: `3`

Min: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Discarded Elements</strong> <code>bool</code></summary>

If enabled, points that don't match any branch are output on the Outside pin. If disabled, unmatched points are omitted entirely.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits point processing capabilities from its base class.

→ See UPCGExPointsProcessorSettings for common processing settings.

### Outputs

| Pin         | Type       | Description                                                               |
| ----------- | ---------- | ------------------------------------------------------------------------- |
| **0 →**     | Point Data | Points that matched Branch 0's filters                                    |
| **1 →**     | Point Data | Points that matched Branch 1's filters                                    |
| **N →**     | Point Data | Points that matched Branch N's filters (dynamic)                          |
| **Outside** | Point Data | Points that matched no branch (when Output Discarded Elements is enabled) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExUberFilterCascade.h)
