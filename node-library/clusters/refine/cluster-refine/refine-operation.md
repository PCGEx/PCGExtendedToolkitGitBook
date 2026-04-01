---
description: >-
  Abstract base class for all edge refinement operations used with the Refine
  Clusters node.
icon: arrow-down-from-arc
---

# Refine Operation

### Overview

Edge Refine Operations are sub-nodes that define how edges should be filtered or removed from a cluster during the refinement process. Each operation implements a specific algorithm or criterion for determining which edges to keep or remove. Operations can process edges individually, process nodes (examining all edges at each vertex), or perform batch operations on the entire cluster.

### How It Works

1. **Preparation**: The operation receives the cluster data and optional heuristics handler.
2. **Processing**: Depending on the operation type, it either:
   * Processes each edge individually (e.g., filter-based, line trace)
   * Processes each node and its connected edges (e.g., keep shortest/longest)
   * Processes the entire cluster at once (e.g., MST algorithms)
3. **Edge Marking**: Edges are marked as valid or invalid based on the operation's logic.
4. **Cleanup**: Invalid edges are removed from the output cluster.

### Available Operations

#### Filter-Based

* Refine : Filter - Remove edges based on filter evaluation

#### Length-Based

* Keep Shortest - Keep only the shortest edge at each vertex
* Keep Longest - Keep only the longest edge at each vertex
* Remove Shortest - Remove the shortest edge at each vertex
* Remove Longest - Remove the longest edge at each vertex

#### Score-Based

* Keep Highest Score - Keep only the highest-scoring edge (requires heuristics)
* Keep Lowest Score - Keep only the lowest-scoring edge (requires heuristics)
* Remove Highest Score - Remove the highest-scoring edge (requires heuristics)
* Remove Lowest Score - Remove the lowest-scoring edge (requires heuristics)

#### Graph Algorithms

* Refine : Gabriel - Produce a Gabriel graph
* Refine : Prim MST - Produce a minimum spanning tree
* Refine : Skeleton - Extract the graph skeleton

#### Topology-Based

* Remove Leaves - Remove dead-end edges
* Remove Leaves Recursive - Recursively remove dead-ends
* Remove Overlap - Remove overlapping edges

#### World Interaction

* Refine : Line Trace - Remove edges that hit world collision

### Settings

This abstract base class has no configurable settings. See the specific operation implementations for their individual settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineOperation.h)
