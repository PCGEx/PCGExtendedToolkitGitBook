---
description: Compute centrality (betweenness, closeness, degree, eigenvector, katz).
icon: share-nodes
---

# Cluster : Centrality

### Overview

This node computes a centrality score for each vertex in a cluster, quantifying how structurally important each vertex is within the graph. Different algorithms capture different notions of importance: whether a vertex acts as a bridge between regions (betweenness), sits close to everything (closeness), has many direct connections (degree), or is connected to other well-connected vertices (eigenvector/katz). The six available algorithms fall into three families -- path-based, local, and iterative -- each revealing different aspects of graph structure. The computed score is written to a configurable output attribute, with optional normalization and contrast adjustment.

### How It Works

1. **Algorithm Selection**: Choose a centrality type that defines the importance metric.
2. **Score Computation**: The selected algorithm evaluates every vertex in the cluster:
   * _Path-based_ (Betweenness, Closeness, Harmonic Closeness): runs shortest-path queries using connected heuristic sub-nodes to weight edges.
   * _Local_ (Degree): counts direct connections per vertex.
   * _Iterative_ (Eigenvector, Katz): repeatedly refines scores until convergence.
3. **Normalization**: Optionally scales all scores to the 0-1 range relative to the highest value.
4. **Contrast**: Optionally reshapes the value distribution using a contrast curve.
5. **Output**: Writes the final score to the specified attribute on each vertex.

**Usage Notes**

* **Heuristics Input**: Path-based centrality types (Betweenness, Closeness, Harmonic Closeness) require heuristic sub-nodes connected to the Heuristics pin. These control edge weighting during shortest-path computation. Degree, Eigenvector, and Katz do not use heuristics.
* **Downsampling**: For large clusters, path-based centrality can be approximated by computing on a random subset of source nodes, trading precision for speed.
* **Attribute Output**: The result is a single `double` attribute per vertex. Use normalization and contrast to shape the distribution for downstream use.

### Behavior

**All six centrality types on the same graph:**

```
Graph:
  A───B───C───D
      │
      E───F

                    A     B     C     D     E     F
                   ───   ───   ───   ───   ───   ───
Degree:             1     3     2     1     2     1
Betweenness:        0     6     3     0     3     0
Closeness:         .125  .200  .167  .100  .167  .125
Harmonic:          1.83  3.17  2.67  1.83  2.67  1.83
Eigenvector:       .27   .58   .49   .22   .49   .27
Katz (α=0.1):      .30   .62   .51   .23   .51   .30
```

**Degree** -- counts direct connections. B has 3 edges (to A, C, E), so it scores highest. This is purely local: a vertex deep in a chain scores the same as one at a critical junction, as long as they have the same number of edges.

**Betweenness** -- counts how many shortest paths between other vertex pairs pass through this vertex. B scores highest because it sits on every shortest path between the upper branch (A) and the lower branch (E, F), and between the left and right sides. D and F score 0 because they are leaf nodes -- no shortest path needs to go through them. Vertices with high betweenness act as bridges or gatekeepers between graph regions.

**Closeness** -- measures how easily a vertex can reach all others, computed as 1 divided by the sum of shortest distances to every other vertex. B scores highest because it has the lowest total distance to all other vertices. Closeness identifies the most "accessible" positions in the graph. Note: closeness produces undefined results if the graph has disconnected components (infinite distances).

**Harmonic Closeness** -- similar to closeness but sums the _inverse_ of each individual distance (1/d₁ + 1/d₂ + ...) rather than inverting the total sum. This handles disconnected components gracefully: unreachable vertices contribute 0 instead of making the result undefined. The relative ranking is similar to closeness on connected graphs but the raw values differ.

**Eigenvector** -- a vertex scores high not just for having many connections, but for being connected to other high-scoring vertices. This is computed iteratively: each vertex's score is proportional to the sum of its neighbors' scores, refined until convergence. In this example, C and E score similarly to each other (both have degree 2), but C edges slightly ahead because one of its neighbors (B) has a higher score than F. Eigenvector captures "prestige" -- importance through association.

**Katz** -- similar to eigenvector but considers _all_ paths between vertices, not just direct connections, with an exponential decay factor (alpha) controlling how quickly distant paths lose influence. Every vertex also receives a baseline score, so isolated vertices are never exactly zero. Lower alpha values make Katz behave more like degree (emphasizing local connections), while higher values approach eigenvector-like behavior (emphasizing global structure).

**Normalization and Contrast:**

```
Raw betweenness: [0, 6, 3, 0, 3, 0]

Normalized (0-1): [0, 1.0, 0.5, 0, 0.5, 0]

OneMinus:         [1.0, 0, 0.5, 1.0, 0.5, 1.0]

Contrast (SCurve, Amount=2.0):
  Values near 0 and 1 are pushed further apart,
  mid-range values shift toward extremes.
```

### Inputs

| Pin            | Type   | Description                                                                  |
| -------------- | ------ | ---------------------------------------------------------------------------- |
| **Vtx**        | Points | Cluster vertices                                                             |
| **Edges**      | Points | Cluster edges                                                                |
| **Heuristics** | Params | Heuristic sub-nodes for edge weighting (used by path-based centrality types) |

### Settings

<details>

<summary><strong>Centrality Type</strong> <code>EPCGExCentralityType</code></summary>

The centrality algorithm to compute. Each captures a different notion of structural importance (see Behavior section for detailed comparison).

| Option                 | Description                                                                                                                                                                 |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Betweenness**        | How often a vertex lies on shortest paths between others. Identifies bridges and gatekeepers between graph regions (Brandes' algorithm, path-based)                         |
| **Closeness**          | How easily a vertex can reach all others, as the inverse of total shortest-path distance. Identifies the most accessible positions. Requires a connected graph (path-based) |
| **Harmonic Closeness** | Like closeness but sums the inverse of each individual distance rather than inverting the total. Handles disconnected components gracefully (path-based)                    |
| **Degree**             | Direct connection count. Purely local -- ignores broader graph structure. Fast to compute                                                                                   |
| **Eigenvector**        | A vertex scores high when connected to other high-scoring vertices. Captures recursive importance through association (iterative, converges via power iteration)            |
| **Katz**               | Considers all paths with exponential decay controlled by alpha. Blends local connectivity with global reach, more tunable than eigenvector (iterative)                      |

Default: `Betweenness`

</details>

<details>

<summary><strong>Heuristic Score Mode</strong> <code>EPCGExHeuristicScoreMode</code></summary>

How multiple heuristic scores are combined when evaluating edge weights.

| Option               | Description                                                       |
| -------------------- | ----------------------------------------------------------------- |
| **Weighted Average** | Average weighted by each heuristic's weight factor                |
| **Geometric Mean**   | Nth root of the product of all scores                             |
| **Weighted Sum**     | Sum of all weighted scores                                        |
| **Harmonic Mean**    | Reciprocal of the average of reciprocals, sensitive to low values |
| **Min**              | Use the lowest score among all heuristics                         |
| **Max**              | Use the highest score among all heuristics                        |

Default: `Weighted Average`

📋 _Visible when Centrality Type = Betweenness, Closeness, or Harmonic Closeness_

</details>

<details>

<summary><strong>Centrality Value Attribute Name</strong> <code>FName</code></summary>

Name of the output attribute written to each vertex containing the computed centrality score.

Default: `Centrality`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normalize</strong> <code>bool</code></summary>

When enabled, scales all centrality scores to the 0-1 range relative to the highest value in each cluster.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>OneMinus</strong> <code>bool</code></summary>

Inverts the normalized value (1 minus score), so that the most central vertices get the lowest values instead of the highest.

Default: `false`

⚡ PCG Overridable

📋 _Visible when Normalize is enabled_

</details>

<details>

<summary><strong>Contrast</strong> <code>bool</code></summary>

When enabled, applies a contrast curve to reshape the value distribution, pushing values toward extremes or compressing them toward the middle.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Curve</strong> <code>EPCGExContrastCurve</code></summary>

The contrast curve type to apply.

| Option     | Description                                                      |
| ---------- | ---------------------------------------------------------------- |
| **Power**  | Simple power curve                                               |
| **SCurve** | Smooth S-shaped curve that pushes mid-range values toward 0 or 1 |
| **Gain**   | Symmetric gain curve centered at 0.5                             |

Default: `SCurve`

⚡ PCG Overridable

📋 _Visible when Contrast is enabled_

</details>

<details>

<summary><strong>Amount</strong> <code>double</code></summary>

Contrast intensity. 1.0 produces no change, values greater than 1 increase contrast, values less than 1 reduce it.

Default: `1.5`

⚡ PCG Overridable

📋 _Visible when Contrast is enabled_

</details>

<details>

<summary><strong>Max Iterations</strong> <code>int32</code></summary>

Maximum number of iterations for iterative centrality algorithms. The computation stops when this limit is reached or when the change between iterations falls below the tolerance.

Default: `100`

⚡ PCG Overridable

📋 _Visible when Centrality Type = Eigenvector or Katz_

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Convergence threshold for iterative algorithms. Iteration stops when the score change between steps is smaller than this value.

Default: `1e-6`

⚡ PCG Overridable

📋 _Visible when Centrality Type = Eigenvector or Katz_

</details>

<details>

<summary><strong>Katz Alpha</strong> <code>double</code></summary>

Attenuation factor for Katz centrality. Controls how quickly the influence of distant paths decays. Must be less than 1/lambda\_max (the largest eigenvalue of the adjacency matrix). Smaller values emphasize local connections, larger values give more weight to distant paths.

Default: `0.1`

⚡ PCG Overridable

📋 _Visible when Centrality Type = Katz_

</details>

<details>

<summary><strong>Downsampling Mode</strong> <code>EPCGExCentralityDownsampling</code></summary>

Strategy to reduce computation time for path-based centrality on large clusters by computing on a subset of source nodes.

| Option           | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| **None**         | Use all vertices as sources (full precision)                 |
| **Random Ratio** | Use a random subset of vertices as sources                   |
| **Filters**      | Use vertex filters to select which vertices serve as sources |

Default: `None`

📋 _Visible when Centrality Type = Betweenness, Closeness, or Harmonic Closeness_

</details>

<details>

<summary><strong>Ratio</strong> <code>FPCGExRandomRatioDetails</code></summary>

Configuration for random downsampling. Controls the proportion of vertices used as source nodes, with optional min/max clamping on the sample count.

⚡ PCG Overridable

📋 _Visible when Downsampling Mode = Random Ratio_

</details>

### Outputs

| Pin       | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| **Vtx**   | Points | Cluster vertices with centrality attribute |
| **Edges** | Points | Cluster edges (pass-through)               |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExClusterCentrality.h)
