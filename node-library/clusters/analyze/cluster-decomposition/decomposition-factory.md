---
description: Base factory for cluster decomposition operations.
icon: arrow-down-from-arc
---

# Decomposition Factory

### Overview

Abstract base class for all decomposition sub-nodes. Each decomposition operation receives a cluster and produces a mapping of node index to cell ID, partitioning the cluster's vertices into discrete groups. Concrete implementations define the decomposition algorithm.

This factory is consumed by the Cluster : Decomposition node.

### How It Works

1. **Preparation**: The operation receives a cluster and optional heuristics data. Octrees are built on demand if the algorithm requires spatial queries.
2. **Decomposition**: The algorithm assigns each cluster node to a cell ID, producing a flat mapping and total cell count.
3. **Output**: The parent node uses the cell ID mapping to write attributes or partition the cluster.

### Settings

This is an abstract base class with no user-facing settings. See the concrete decomposition types for configurable options:

* BSP Occupancy
* Convex BSP
* Grid Partition
* Max Boxes
* Max Boxes (Extended)
* Spectral
* Threshold

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Core/PCGExDecompositionOperation.h)
