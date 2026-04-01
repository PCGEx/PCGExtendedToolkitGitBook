---
description: Bidirectional search algorithm that searches from both seed and goal.
icon: function
---

# Search : Bidirectional

### Overview

Bidirectional search runs two simultaneous searches: one from the seed toward the goal, and one from the goal toward the seed. When the two searches meet, the path is found. This approach can be significantly faster than unidirectional search for large graphs, reducing time complexity from O(b^d) to approximately O(b^(d/2)) where b is the branching factor and d is the path depth.

### How It Works

1. **Initialize Both Ends**: Start searches from seed and goal simultaneously.
2. **Alternate Expansion**: Expand nodes from the smaller frontier.
3. **Check Meeting**: Detect when a node is visited by both searches.
4. **Merge Paths**: Combine forward and backward paths at the meeting point.
5. **Return Result**: Output the complete path from seed to goal.

**Usage Notes**

* **Large Graphs**: Most effective when the graph is large and paths are long.
* **Symmetric Cost**: Works best when edge costs are symmetric.
* **Memory Trade-off**: Uses more memory than unidirectional but runs faster.
* **Early Termination**: Stops as soon as the searches meet.

### Behavior

**Bidirectional Search Process:**

```
Cluster graph:
    [A]───[B]───[C]───[D]───[E]
     │     │     │     │     │
    [F]───[G]───[H]───[I]───[J]

Seed: A, Goal: J

Forward search (from A):
   Step 1: A → {B, F}
   Step 2: B,F → {C, G}
   Step 3: C,G → {D, H}

Backward search (from J):
   Step 1: J → {E, I}
   Step 2: E,I → {D, H}

Meeting detected at D or H!

Path: A → B → C → D → I → J
(Assembled from forward + backward paths)
```

### Settings

This algorithm has no additional settings beyond those inherited from the base search algorithm.

#### Inherited Settings

→ See Search Algorithm : Base for Early Exit setting.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Search/PCGExSearchBidirectional.h)
