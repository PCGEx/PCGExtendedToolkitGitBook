---
description: Dijkstra's algorithm for finding shortest paths through weighted graphs.
icon: function
---

# Search : Dijkstra

### Overview

Dijkstra's algorithm finds the shortest path between nodes by exploring all paths in order of their cumulative cost. Unlike A\*, it does not use heuristics to guide the search, which means it explores more nodes but is more respectful of edge weights and modifiers. This makes it suitable when heuristic estimates might be unreliable or when you want paths that strictly minimize total cost.

### How It Works

1. **Initialize**: Set seed distance to 0, all others to infinity.
2. **Select Minimum**: Pick the unvisited node with smallest distance.
3. **Update Neighbors**: For each neighbor, update distance if shorter path found.
4. **Mark Visited**: Current node is now finalized.
5. **Repeat**: Continue until goal is reached or all nodes visited.

**Usage Notes**

* **No Heuristics**: Explores based purely on actual path cost, not estimates.
* **Weight Sensitive**: More accurately respects edge weights than A\*.
* **Complete**: Guarantees finding the shortest path if one exists.
* **Slower**: Explores more nodes than A\* but produces more consistent results.

### Behavior

**Dijkstra Search Process:**

```
Cluster with edge weights:
    [A]──2──[B]──3──[C]
     │       │       │
     4       1       2
     │       │       │
    [D]──1──[E]──1──[F]

Seed: A, Goal: F

Expansion order (by distance from A):
   Step 1: A (dist=0)
   Step 2: B (dist=2)
   Step 3: E (dist=3, via B)
   Step 4: D (dist=4)
   Step 5: F (dist=4, via E)

Path: A → B → E → F (cost: 4)

Note: Unlike A*, Dijkstra doesn't estimate
remaining distance - it explores all paths
with cost ≤ current best.
```

### Settings

This algorithm has no additional settings beyond those inherited from the base search algorithm.

#### Inherited Settings

→ See Search Algorithm : Base for Early Exit setting.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Search/PCGExSearchDijkstra.h)
