---
description: Refines a cluster into a minimum spanning tree using Prim's algorithm.
icon: function
---

# Refine : MST (Prim)

### Overview

This edge refinement operation produces a Minimum Spanning Tree (MST) from the input cluster using Prim's algorithm. The MST is a subset of edges that connects all vertices with the minimum total edge weight (as determined by heuristics) while containing no cycles. The result is a tree structure that spans all vertices with the lowest possible total cost.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Initialization**: Starts from a seed vertex determined by the heuristics handler.
2. **Priority Queue**: Maintains a queue of edges sorted by their heuristic score.
3. **Greedy Selection**: Repeatedly selects the lowest-cost edge that connects to an unvisited vertex.
4. **Tree Building**: Adds selected edges to the MST until all vertices are connected.
5. **Edge Marking**: Only edges that are part of the MST are kept; all others are removed.

**Usage Notes**

* **Requires Heuristics**: This operation requires heuristics to determine edge weights/costs.
* **Connected Output**: The result is always a connected tree (assuming the input cluster is connected).
* **No Cycles**: The MST contains exactly N-1 edges for N vertices, with no cycles.
* **Seed Dependent**: The starting seed can affect which specific MST is produced when multiple edges have equal weights.

### Behavior

```
Before (weighted graph):        After (MST):

    A---3---B                   A-------B
    |\     /|                   |
   1| \5  / |2                  |
    |  \ /  |                   |
    C---4---D                   C-------D

Edges kept: A-C(1), B-D(2), A-B(3) = total 6
Edges removed: A-D(5), C-D(4)
```

### Settings

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the refinement result. When enabled, edges that are NOT part of the MST are kept, while MST edges are removed.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefinePrimMST.h)
