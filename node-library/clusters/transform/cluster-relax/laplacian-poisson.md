---
description: >-
  Simple smoothing relaxation that moves each vertex toward the centroid of its
  connected neighbors.
icon: function
---

# Laplacian (Poisson)

### Overview

This relaxation operation implements Laplacian smoothing, one of the simplest and most effective mesh/graph smoothing algorithms. Each vertex moves toward the average position of its neighbors, gradually evening out vertex distribution along the cluster topology. The result is a smoother, more uniform layout that preserves the overall shape while reducing local irregularities.

> This is a [relax-operation.md](relax-operation.md "mention")

### How It Works

1. **Neighbor Centroid**: For each vertex, calculates the centroid (average position) of all connected neighbors.
2. **Displacement**: Computes the vector from the current position toward the centroid.
3. **Position Update**: Moves the vertex toward the centroid by the full displacement divided by neighbor count.
4. **Iteration**: Repeats to progressively smooth the layout.

**Usage Notes**

* **Shrinkage**: Pure Laplacian smoothing tends to shrink the graph toward its center over many iterations.
* **Fast Convergence**: Typically converges quickly compared to force-directed methods.
* **Topology Preserving**: Only moves vertices along their neighbor relationships — doesn't create new connections.
* **Uniform Spacing**: Naturally produces more uniform edge lengths along chains.

### Behavior

```
Before:                         After (1 iteration):

    A                           A
     \                           \
      B--C                        B--C
         |                           |
    D----E                      D----E

Each vertex moves toward its neighbors' average position.
Irregular spacing becomes more uniform.
```

### Settings

This relaxation operation has no additional settings beyond the inherited base class settings. Its simplicity is its strength — it provides predictable smoothing behavior without tuning parameters.

#### Inherited Settings

This operation inherits common relaxation settings from its base class, including iteration count and influence/strength.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExLaplacianRelax.h)
