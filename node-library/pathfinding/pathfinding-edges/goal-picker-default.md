---
description: Default goal picker that pairs seeds with goals by index.
icon: function
---

# Goal Picker : Default

### Overview

This is the base goal picker used by pathfinding nodes. It pairs each seed point with a goal point based on their index positions. The index safety setting controls how out-of-range indices are handled when there are more seeds than goals.

### How It Works

1. **Read Seed Index**: Gets the index of the current seed point.
2. **Apply Safety**: Maps the index using the safety mode (tile, clamp, etc.).
3. **Return Goal**: Returns the goal at the resulting index.

**Usage Notes**

* **Index-Based**: Seed 0 pairs with Goal 0, Seed 1 with Goal 1, etc.
* **Index Safety**: When seed count exceeds goal count, the safety mode determines mapping.
* **Single Goal**: Each seed produces one path to its paired goal.

### Behavior

```
Index-Based Pairing:

Seeds: [S0, S1, S2, S3, S4]
Goals: [G0, G1, G2]

Index Safety = Tile:
   S0 → G0, S1 → G1, S2 → G2, S3 → G0, S4 → G1

Index Safety = Clamp:
   S0 → G0, S1 → G1, S2 → G2, S3 → G2, S4 → G2

Index Safety = Yoyo:
   S0 → G0, S1 → G1, S2 → G2, S3 → G1, S4 → G0
```

### Settings

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle seed indices that exceed the goal count.

| Option     | Description                                     |
| ---------- | ----------------------------------------------- |
| **Ignore** | Invalid indices are skipped (no path created)   |
| **Tile**   | Wraps around using modulo (0, 1, 2, 0, 1, 2...) |
| **Clamp**  | Clamps to last valid index                      |
| **Yoyo**   | Bounces back and forth (0, 1, 2, 1, 0, 1...)    |

Default: `Tile`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/GoalPickers/PCGExGoalPicker.h)
