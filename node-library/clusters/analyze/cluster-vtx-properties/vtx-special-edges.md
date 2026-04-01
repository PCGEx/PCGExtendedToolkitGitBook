---
description: Outputs data about a vertex's shortest, longest, and average edges.
icon: circle-dashed
---

# Vtx : Special Edges

### Overview

This vertex property provider analyzes the edges connected to each vertex and writes attribute data for three categories: the shortest edge, the longest edge, and the computed average across all edges. For each category, you can output direction vectors, lengths, edge indices, and vertex indices to attributes.

> This is a [vtx-property-provider.md](vtx-property-provider.md "mention")

### How It Works

1. **Edge Analysis**: For each vertex, the node examines all connected edges and their lengths.
2. **Extrema Detection**: Identifies the shortest and longest edges by comparing distances to neighboring vertices.
3. **Average Calculation**: Computes the average direction and length across all connected edges.
4. **Attribute Output**: Writes the enabled outputs as vertex attributes with configurable names.

**Usage Notes**

* **Per-Vertex Output**: Each vertex receives its own shortest/longest/average values based only on its connected edges.
* **Index Outputs**: The edge and vertex index outputs let you trace back to the specific edge or neighbor vertex for shortest/longest.
* **Average Direction**: The average direction is the normalized sum of all edge directions from this vertex.

### Behavior

```
Input Cluster:                  Output Attributes (per vertex):

    B                           For vertex A with edges to B,C,D:
    |
    | (len=5)                   Shortest: edge to D (len=2)
    |                             → ShortestDir, ShortestLen, ShortestEdgeIndex...
    A----C (len=3)
    |                           Longest: edge to B (len=5)
    | (len=2)                     → LongestDir, LongestLen, LongestEdgeIndex...
    |
    D                           Average: mean of all 3 edges
                                  → AverageDir, AverageLen
```

### Settings

#### Shortest Edge

<details>

<summary><strong>Shortest Edge</strong> <code>FPCGExEdgeOutputWithIndexSettings</code></summary>

Output settings for the shortest edge connected to each vertex.

**Direction Output:**

| Setting                 | Type    | Default       | Description                                                       |
| ----------------------- | ------- | ------------- | ----------------------------------------------------------------- |
| **Write Direction**     | `bool`  | `false`       | Enable to write the direction to the shortest neighbor            |
| **Direction Attribute** | `FName` | `ShortestDir` | Attribute name for the direction vector                           |
| **Invert**              | `bool`  | `false`       | Invert the direction (point away from neighbor instead of toward) |

**Length Output:**

| Setting              | Type    | Default       | Description                              |
| -------------------- | ------- | ------------- | ---------------------------------------- |
| **Write Length**     | `bool`  | `false`       | Enable to write the shortest edge length |
| **Length Attribute** | `FName` | `ShortestLen` | Attribute name for the length value      |

**Index Output:**

| Setting                      | Type    | Default                 | Description                                              |
| ---------------------------- | ------- | ----------------------- | -------------------------------------------------------- |
| **Write Edge Index**         | `bool`  | `false`                 | Enable to write the index of the shortest edge           |
| **Edge Index Attribute**     | `FName` | `ShortestEdgeIndex`     | Attribute name for the edge index                        |
| **Write Vtx Index**          | `bool`  | `false`                 | Enable to write the index of the closest neighbor vertex |
| **Vtx Index Attribute**      | `FName` | `ShortestVtxIndex`      | Attribute name for the vertex index                      |
| **Write Neighbor Count**     | `bool`  | `false`                 | Enable to write the total neighbor count                 |
| **Neighbor Count Attribute** | `FName` | `ShortestNeighborCount` | Attribute name for neighbor count                        |

All settings are PCG Overridable.

</details>

#### Longest Edge

<details>

<summary><strong>Longest Edge</strong> <code>FPCGExEdgeOutputWithIndexSettings</code></summary>

Output settings for the longest edge connected to each vertex.

**Direction Output:**

| Setting                 | Type    | Default      | Description                                            |
| ----------------------- | ------- | ------------ | ------------------------------------------------------ |
| **Write Direction**     | `bool`  | `false`      | Enable to write the direction to the farthest neighbor |
| **Direction Attribute** | `FName` | `LongestDir` | Attribute name for the direction vector                |
| **Invert**              | `bool`  | `false`      | Invert the direction                                   |

**Length Output:**

| Setting              | Type    | Default      | Description                             |
| -------------------- | ------- | ------------ | --------------------------------------- |
| **Write Length**     | `bool`  | `false`      | Enable to write the longest edge length |
| **Length Attribute** | `FName` | `LongestLen` | Attribute name for the length value     |

**Index Output:**

| Setting                      | Type    | Default                | Description                                               |
| ---------------------------- | ------- | ---------------------- | --------------------------------------------------------- |
| **Write Edge Index**         | `bool`  | `false`                | Enable to write the index of the longest edge             |
| **Edge Index Attribute**     | `FName` | `LongestEdgeIndex`     | Attribute name for the edge index                         |
| **Write Vtx Index**          | `bool`  | `false`                | Enable to write the index of the farthest neighbor vertex |
| **Vtx Index Attribute**      | `FName` | `LongestVtxIndex`      | Attribute name for the vertex index                       |
| **Write Neighbor Count**     | `bool`  | `false`                | Enable to write the total neighbor count                  |
| **Neighbor Count Attribute** | `FName` | `LongestNeighborCount` | Attribute name for neighbor count                         |

All settings are PCG Overridable.

</details>

#### Average Edge

<details>

<summary><strong>Average Edge</strong> <code>FPCGExSimpleEdgeOutputSettings</code></summary>

Output settings for the average across all edges connected to each vertex.

**Direction Output:**

| Setting                 | Type    | Default      | Description                                            |
| ----------------------- | ------- | ------------ | ------------------------------------------------------ |
| **Write Direction**     | `bool`  | `false`      | Enable to write the average direction across all edges |
| **Direction Attribute** | `FName` | `AverageDir` | Attribute name for the averaged direction vector       |
| **Invert**              | `bool`  | `false`      | Invert the direction                                   |

**Length Output:**

| Setting              | Type    | Default      | Description                                 |
| -------------------- | ------- | ------------ | ------------------------------------------- |
| **Write Length**     | `bool`  | `false`      | Enable to write the average edge length     |
| **Length Attribute** | `FName` | `AverageLen` | Attribute name for the average length value |

All settings are PCG Overridable.

</details>

#### Inherited Settings

This node inherits common settings from its base class.

See Vtx Property Provider for base class information.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/VtxProperties/PCGExVtxPropertySpecialEdges.h)
