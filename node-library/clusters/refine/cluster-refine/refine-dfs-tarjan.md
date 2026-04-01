---
description: >-
  Identifies bridge edges using Tarjan's DFS algorithm and removes them from the
  cluster.
icon: function
---

# Refine : DFS (Tarjan)

### Overview

This edge refinement operation uses Tarjan's Depth-First Search algorithm to identify bridge edges — edges whose removal would disconnect the graph or split it into more connected components. By default, bridges are removed, which can break a cluster into its 2-edge-connected components. Inverting keeps only bridges, preserving the critical connections that hold the graph together.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **DFS Traversal**: Performs a depth-first search through all vertices, tracking discovery times.
2. **Low-Link Calculation**: For each vertex, calculates the lowest discovery time reachable through back edges.
3. **Bridge Detection**: An edge is a bridge if its endpoint cannot reach any vertex discovered before the edge without using that edge.
4. **Edge Marking**: Bridges are removed by default (or kept if inverted).

**Usage Notes**

* **Bridges**: A bridge is an edge that, if removed, increases the number of connected components in the graph.
* **Redundancy Analysis**: Bridges represent single points of failure in connectivity — removing them identifies isolated regions.
* **Invert for Skeleton**: Enable Invert to keep only bridges, producing a minimal skeleton of critical edges.

### Behavior

```
Before:                         After (bridges removed):

    A---B---C                   A---B   C
    |   |   |                   |   |   |
    D---E---F                   D---E   F

Edge B-C and E-F are bridges (removing them disconnects C-F from the rest).
After refinement: B-C and E-F removed.

With Invert = true: Only B-C and E-F are kept.
```

### Settings

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the refinement result. When disabled (default), bridge edges are removed. When enabled, only bridge edges are kept and all other edges are removed.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineTrajanDFS.h)
