---
description: Each seed creates paths to all goals.
icon: function
---

# Goal Picker : All

### Overview

This goal picker creates paths from each seed point to every goal point. If you have 3 seeds and 5 goals, this produces 15 paths (3 × 5). This is useful for computing full connectivity between sets of points.

### How It Works

1. **Enumerate Goals**: Gets the total count of goal points.
2. **Return All Indices**: For each seed, returns all goal indices.
3. **Create Paths**: The pathfinding node creates one path per seed-goal pair.

**Usage Notes**

* **Multiplicative Output**: Path count = Seeds × Goals.
* **Memory Intensive**: Large seed and goal counts can produce many paths.
* **Full Connectivity**: Every seed connects to every goal.

### Behavior

```
All-Goals Pairing:

Seeds: [S0, S1]
Goals: [G0, G1, G2]

Output Paths:
   S0 → G0
   S0 → G1
   S0 → G2
   S1 → G0
   S1 → G1
   S1 → G2

Total: 2 seeds × 3 goals = 6 paths
```

### Settings

This picker has no additional settings beyond those inherited from the base goal picker.

#### Inherited Settings

→ See Goal Picker : Default for base settings (Index Safety is not used since all goals are selected).

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/GoalPickers/PCGExGoalPickerAll.h)
