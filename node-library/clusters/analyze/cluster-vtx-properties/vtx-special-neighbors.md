---
description: Outputs data about a vertex's most-connected and least-connected neighbors.
icon: circle-dashed
---

# Vtx : Special Neighbors

### Overview

This vertex property provider analyzes the neighbors connected to each vertex and identifies which neighbor has the **highest valency** (most connections) and which has the **lowest valency** (fewest connections). You can output direction vectors, distances, edge indices, and vertex indices for both extremes.

> This is a [vtx-property-provider.md](vtx-property-provider.md "mention")

### How It Works

1. **Neighbor analysis**: For each vertex, examine all connected neighbors and compare their adjacency count (how many edges each neighbor has).
2. **Find extremes**: Identify the neighbor with the most connections (largest) and the one with the fewest (smallest).
3. **Write outputs**: Direction, distance, edge index, vertex index, and neighbor count for each extreme.

{% hint style="info" %}
**Valency, not bounds.** This node compares neighbors by their **connection count** (number of edges), not by their physical size or bounds. The "largest" neighbor is the one with the most edges in the cluster.
{% endhint %}

**Usage Notes**

* **Complementary to Special Edges**: Special Edges finds the longest/shortest edge by length. This node finds the most/least connected neighbor by valency.
* **Index outputs**: Use the edge and vertex index outputs to trace back to specific neighbors for further processing.

### Behavior

```
Input Cluster:                  Output Attributes (per vertex):

    B (4 edges)                 For vertex A with neighbors B,C,D:
    |
    |                           Largest: neighbor B (most connections)
    |                             → LargestDir, LargestLen, LargestVtxIndex...
    A----C (2 edges)
    |                           Smallest: neighbor D (fewest connections)
    |                             → SmallestDir, SmallestLen, SmallestVtxIndex...
    |
    D (1 edge)
```

### Settings

#### Largest Neighbor

<details>

<summary><strong>Largest Neighbor</strong> <code>FPCGExEdgeOutputWithIndexSettings</code></summary>

Output settings for the neighbor with the most connections (highest valency).

**Direction Output:**

| Setting                 | Type    | Default      | Description                                                       |
| ----------------------- | ------- | ------------ | ----------------------------------------------------------------- |
| **Write Direction**     | `bool`  | `false`      | Enable to write the direction to the largest neighbor             |
| **Direction Attribute** | `FName` | `LargestDir` | Attribute name for the direction vector                           |
| **Invert**              | `bool`  | `false`      | Invert the direction (point away from neighbor instead of toward) |

**Length Output:**

| Setting              | Type    | Default      | Description                                          |
| -------------------- | ------- | ------------ | ---------------------------------------------------- |
| **Write Length**     | `bool`  | `false`      | Enable to write the distance to the largest neighbor |
| **Length Attribute** | `FName` | `LargestLen` | Attribute name for the distance value                |

**Index Output:**

| Setting                      | Type    | Default                | Description                                                   |
| ---------------------------- | ------- | ---------------------- | ------------------------------------------------------------- |
| **Write Edge Index**         | `bool`  | `false`                | Enable to write the index of the edge to the largest neighbor |
| **Edge Index Attribute**     | `FName` | `LargestEdgeIndex`     | Attribute name for the edge index                             |
| **Write Vtx Index**          | `bool`  | `false`                | Enable to write the index of the largest neighbor vertex      |
| **Vtx Index Attribute**      | `FName` | `LargestVtxIndex`      | Attribute name for the vertex index                           |
| **Write Neighbor Count**     | `bool`  | `false`                | Enable to write the total neighbor count                      |
| **Neighbor Count Attribute** | `FName` | `LargestNeighborCount` | Attribute name for neighbor count                             |

All settings are PCG Overridable.

</details>

#### Smallest Neighbor

<details>

<summary><strong>Smallest Neighbor</strong> <code>FPCGExEdgeOutputWithIndexSettings</code></summary>

Output settings for the neighbor with the fewest connections (lowest valency).

**Direction Output:**

| Setting                 | Type    | Default       | Description                                            |
| ----------------------- | ------- | ------------- | ------------------------------------------------------ |
| **Write Direction**     | `bool`  | `false`       | Enable to write the direction to the smallest neighbor |
| **Direction Attribute** | `FName` | `SmallestDir` | Attribute name for the direction vector                |
| **Invert**              | `bool`  | `false`       | Invert the direction                                   |

**Length Output:**

| Setting              | Type    | Default       | Description                                           |
| -------------------- | ------- | ------------- | ----------------------------------------------------- |
| **Write Length**     | `bool`  | `false`       | Enable to write the distance to the smallest neighbor |
| **Length Attribute** | `FName` | `SmallestLen` | Attribute name for the distance value                 |

**Index Output:**

| Setting                      | Type    | Default                 | Description                                                    |
| ---------------------------- | ------- | ----------------------- | -------------------------------------------------------------- |
| **Write Edge Index**         | `bool`  | `false`                 | Enable to write the index of the edge to the smallest neighbor |
| **Edge Index Attribute**     | `FName` | `SmallestEdgeIndex`     | Attribute name for the edge index                              |
| **Write Vtx Index**          | `bool`  | `false`                 | Enable to write the index of the smallest neighbor vertex      |
| **Vtx Index Attribute**      | `FName` | `SmallestVtxIndex`      | Attribute name for the vertex index                            |
| **Write Neighbor Count**     | `bool`  | `false`                 | Enable to write the total neighbor count                       |
| **Neighbor Count Attribute** | `FName` | `SmallestNeighborCount` | Attribute name for neighbor count                              |

All settings are PCG Overridable.

</details>

#### Inherited Settings

This node inherits common settings from its base class.

See Vtx Property Provider for base class information.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/VtxProperties/PCGExVtxPropertySpecialNeighbors.h)
