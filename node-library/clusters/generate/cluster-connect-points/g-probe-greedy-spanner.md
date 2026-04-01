---
description: Greedy t-spanner - sparse graph with path length guarantees.
icon: circle-dashed
---

# G-Probe : Greedy Spanner

### Overview

This global probe creates a t-spanner graph, a sparse connectivity structure that guarantees the shortest path between any two points through the graph is at most t times their direct Euclidean distance. The greedy algorithm iteratively adds edges, skipping any edge where a sufficiently short path already exists. This produces efficient networks that balance connectivity with edge economy.

<figure><img src="../../../../.gitbook/assets/image (292).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Edge Sorting**: Generates all potential edges and sorts them by length (shortest first)
2. **Greedy Selection**: For each candidate edge, checks if a path of length ≤ t × direct distance already exists
3. **Edge Addition**: Adds the edge only if no such path exists, ensuring the spanner property is maintained
4. **Graph Building**: Continues until all edges are processed or the candidate limit is reached

**Usage Notes**

* **Stretch Factor (t)**: A value of 2.0 means paths through the graph are at most twice the straight-line distance. Lower values (closer to 1.0) create denser graphs with more direct routes; higher values create sparser graphs
* **Computational Cost**: The greedy spanner algorithm runs Dijkstra's algorithm for each candidate edge, making it more expensive than simpler probes. Use MaxEdgeCandidates to limit processing time
* **Network Efficiency**: Spanners are ideal for creating transportation or communication networks that minimize total edge length while maintaining reasonable path quality

### Behavior

```
Greedy Spanner Construction (t=2.0):

    All possible edges:          t-Spanner result:

       •─────•─────•               •─────•─────•
       │╲   ╱│╲   ╱│               │     │     │
       │ ╲ ╱ │ ╲ ╱ │               │     │     │
       │  ╳  │  ╳  │       →       │     │     │
       │ ╱ ╲ │ ╱ ╲ │               │     │     │
       │╱   ╲│╱   ╲│               │     │     │
       •─────•─────•               •─────•─────•

    Dense connectivity           Sparse but navigable
    (many redundant edges)       (path ≤ 2× direct distance)


    Path guarantee example (t=2):

         A ─────────── B          Direct distance: 100
              100

         A ─── C ─── B            Spanner path: ≤ 200
           60    60               (60 + 60 = 120 ≤ 200 ✓)
```

### Settings

#### Spanner Configuration

<details>

<summary><strong>Stretch Factor</strong> <code>double</code></summary>

The maximum allowed ratio between graph path distance and direct Euclidean distance (the "t" in t-spanner). Lower values create denser graphs with more direct paths; higher values create sparser graphs with potentially longer routes.

* **1.0**: Would require all direct edges (not sparse)
* **2.0**: Paths can be up to 2× the direct distance
* **3.0+**: Very sparse, paths can be significantly longer

Default: `2.0`

Range: `1.0` - `10.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Edge Candidates</strong> <code>int32</code></summary>

Maximum number of potential edges to evaluate. The algorithm processes edges shortest-first, so this limit affects which long edges might be skipped. Higher values produce more complete spanners but take longer to compute.

Default: `50000`

Min: `100`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory to connect to Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeSpanner.h)
