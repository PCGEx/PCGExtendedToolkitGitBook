---
description: >-
  Refines a cluster into a β-skeleton (beta-skeleton) graph using a configurable
  neighborhood parameter.
icon: function
---

# Refine : β Skeleton

### Overview

This edge refinement operation produces a β-skeleton graph, a family of geometric graphs that generalizes several proximity structures. The Beta parameter controls how "empty" the region around each edge must be for the edge to be kept. Lower Beta values produce denser graphs; higher values produce sparser graphs. At Beta = 1, this produces a Gabriel graph.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Edge Iteration**: Processes each edge in the cluster.
2. **Forbidden Region**: Defines a region around the edge based on the Beta parameter:
   * **Beta ≤ 1 (Lune-based)**: The forbidden region is the intersection of two circles centered on the endpoints with radius = edge\_length / Beta.
   * **Beta > 1 (Circle-based)**: The forbidden region consists of two circles offset perpendicular to the edge.
3. **Emptiness Test**: Checks if any other vertex falls within the forbidden region.
4. **Edge Removal**: If a vertex is found in the forbidden region, the edge is removed.

**Usage Notes**

* **Beta = 1**: Produces a Gabriel graph (equivalent to the Gabriel refinement operation).
* **Beta < 1**: Produces denser graphs with more edges (approaching Delaunay triangulation as Beta → 0).
* **Beta > 1**: Produces sparser graphs (approaching the relative neighborhood graph as Beta → ∞).
* **Neighborhood Control**: Use Beta to tune the balance between connectivity and sparseness.

### Behavior

```
Beta-Skeleton Test (Beta ≤ 1):

    Edge A----B
         ·
        (C)   ← If C is inside the lune formed by
                circles of radius (dist/Beta) centered
                at A and B, edge A-B is removed.

Lower Beta = smaller forbidden region = more edges kept
Higher Beta = larger forbidden region = fewer edges kept
```

### Settings

<details>

<summary><strong>Beta</strong> <code>double</code></summary>

The Beta parameter controlling the size of the forbidden region around each edge.

* **Beta ≤ 1**: Uses lune-based (intersection of circles) test. Smaller values keep more edges.
* **Beta > 1**: Uses circle-based test with circles offset perpendicular to the edge. Larger values produce sparser graphs.
* **Beta = 1**: Equivalent to Gabriel graph.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the refinement result. When enabled, edges that would normally be removed (those with vertices in their forbidden region) are kept, and edges that would be kept are removed.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineSkeleton.h)
