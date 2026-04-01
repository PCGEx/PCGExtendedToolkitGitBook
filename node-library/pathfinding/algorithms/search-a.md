---
description: A* search algorithm for finding optimal paths through cluster graphs.
icon: function
---

# Search : A\*

### Overview

A\* (A-star) is a best-first search algorithm that finds the shortest path between two points by combining the actual cost from the start with a heuristic estimate of the remaining distance to the goal. This allows A\* to explore fewer nodes than uninformed algorithms while still guaranteeing the shortest path (when using admissible heuristics).

### How It Works

1. **Initialize**: Start from the seed node with zero cost.
2. **Expand Best**: Select the node with lowest f(n) = g(n) + h(n).
3. **Update Neighbors**: Calculate costs for adjacent unvisited nodes.
4. **Check Goal**: If goal reached, reconstruct path.
5. **Repeat**: Continue until goal found or all nodes explored.

**Usage Notes**

* **Optimal Paths**: Guarantees shortest path when heuristics don't overestimate.
* **Early Exit**: Stops as soon as the goal is reached (configurable).
* **Heuristic-Dependent**: Quality depends on the heuristics provided.
* **Memory Efficient**: Only keeps track of the frontier and visited nodes.

### Behavior

**A\* Search Process:**

```
Cluster with costs (node positions affect heuristic):
    [A]──2──[B]──3──[C]
     │       │       │
     4       1       2
     │       │       │
    [D]──1──[E]──1──[F]

Seed: A, Goal: F

Search (f = g + h, where h is distance to F):
   Step 1: Expand A → neighbors B(g=2), D(g=4)
   Step 2: Expand B (lower f) → neighbor E(g=3)
   Step 3: Expand E → neighbor F(g=4)
   Step 4: Goal reached!

Path: A → B → E → F (cost: 4)
```

### Settings

This algorithm has no additional settings beyond those inherited from the base search algorithm.

#### Inherited Settings

→ See Search Algorithm : Base for Early Exit setting.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/Search/PCGExSearchAStar.h)
