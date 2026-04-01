---
description: Partition clusters using graph Laplacian spectral bisection.
icon: function
---

# Decomp : Spectral

### Overview

Computes the graph Laplacian of the cluster and finds the Fiedler vector (second smallest eigenvector) to bisect the graph. Recursive bisection produces the target number of partitions. This is a topology-aware decomposition: it minimizes the number of edges cut between cells, so nodes that are well-connected stay together even if they're spatially distant.

### How It Works

1. **Laplacian Construction**: Builds the graph Laplacian L = D - A, where D is the degree matrix and A is the weighted adjacency matrix. Edge weights come from heuristics if available, otherwise default to 1.0.
2. **Fiedler Vector**: Uses shifted power iteration to find the second smallest eigenvector of L (the Fiedler vector). The shift converts the "smallest" problem into a "largest" problem for stable convergence.
3. **Bisection**: Nodes with positive Fiedler values go to one partition, negative to the other. This split minimizes edge cuts across the boundary.
4. **Recursive Bisection**: Each half is recursively bisected until the target number of partitions is reached.

**Usage Notes**

* **Heuristics**: If a Heuristics sub-node is connected to the parent Decomposition node, edge weights are derived from heuristic scores. This lets you influence which edges are "cheap" to cut.
* **Power of 2**: Num Partitions works best as a power of 2, since the algorithm uses recursive bisection. Non-power-of-2 values still work but may produce uneven partition sizes.
* **Disconnected Clusters**: If the cluster has disconnected components, the Fiedler computation may not converge cleanly. Each connected component is effectively treated as its own partition.

### Settings

<details>

<summary><strong>Num Partitions</strong> <code>int32</code></summary>

Number of partitions to produce. Powers of 2 give the most balanced results.

Default: `2`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Iterations</strong> <code>int32</code></summary>

Maximum iterations for power iteration convergence.

Default: `200`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Convergence Tolerance</strong> <code>double</code></summary>

Convergence tolerance for eigenvector computation. Smaller values give more precise partitions at the cost of more iterations.

Default: `1e-6`

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompSpectral.h)
