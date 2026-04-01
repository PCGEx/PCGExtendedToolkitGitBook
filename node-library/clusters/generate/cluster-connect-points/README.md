---
description: Connect points according to a set of probes.
icon: circle-nodes
---

# Cluster : Connect Points

### Overview

Connect Points creates cluster graphs by connecting input points based on configurable probe rules. Each point queries neighboring points using one or more probe operations to determine which connections to establish. The result is a cluster with vertices (the original points) and edges representing the connections.

<figure><img src="../../../../.gitbook/assets/image (280).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Filter Evaluation**: Evaluates Generator and Connectable filters to determine which points participate in each role.
2. **Probe Setup**: Loads probe factories from the input pin to define connection rules.
3. **Spatial Indexing**: Builds an octree for efficient neighbor queries (if needed by probes).
4. **Connection Testing**: Each generator point uses the configured probes to find candidate connectable points.
5. **Edge Deduplication**: Removes duplicate edges to produce a clean graph.
6. **Graph Output**: Outputs vertices and edges as a cluster data structure.

**Usage Notes**

* **Probe Factories Required**: At least one probe factory must be connected to the input pin for the node to produce edges.
* **Probe Combination**: Multiple probes can be combined - each probe type contributes its own connection logic.
* **Generator vs Connectable**: Use the filter pins to create asymmetric connectivity — for example, only certain "hub" points generate connections while all points can receive them.
* **2D Projection**: When enabled, connection tests ignore the projected axis, useful for top-down or side-view connections.

### Behavior

```
Input Points + Probes (no filters):

  ●A        ●B        ●C        ●D

With KNN Probe (K=2):

  ●A────────●B────────●C────────●D
      edge      edge      edge

Output: Cluster with 4 vertices, 3 edges


With Generator Filter (only B can generate) + Connectable Filter (all can receive):

  ●A        ●B        ●C        ●D
      ←─────┼─────→
            │
  ●A────────●B────────●C
      edge      edge

Only B creates edges; A, C, D can receive but not initiate.
```

### Inputs

| Pin                     | Type             | Description                                                                                                   |
| ----------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------- |
| **In**                  | Point Data       | Points to connect into clusters                                                                               |
| **Probes**              | Probe Factories  | One or more probe configurations defining connection rules                                                    |
| **Generator Filters**   | Filter Factories | Points that pass these filters can generate (create) connections. Points that fail won't initiate any edges.  |
| **Connectable Filters** | Filter Factories | Points that pass these filters can receive connections. Points that fail won't be connected to by generators. |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Prevent Coincidence</strong> <code>bool</code></summary>

When enabled, prevents connections between points that are at the same position (within tolerance). This avoids zero-length edges from duplicate points.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Coincidence Tolerance</strong> <code>double</code></summary>

Distance threshold below which two points are considered coincident and won't be connected.

Default: `0.001`

📋 _Visible when Prevent Coincidence is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Project Points</strong> <code>bool</code></summary>

When enabled, projects points to 2D for connection testing. Useful when you want to ignore height differences when determining connections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Project Points (Settings)</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Projection settings for 2D connection testing.

📋 _Visible when Project Points is enabled_

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Settings controlling how the output cluster graph is built.

| Property                      | Type                | Default      | Description                                                 |
| ----------------------------- | ------------------- | ------------ | ----------------------------------------------------------- |
| **Write Edge Position**       | `bool`              | `true`       | Write edge midpoint position attribute                      |
| **Edge Position**             | `double`            | `0.5`        | Normalized position along edge (0=start, 1=end)             |
| **Remove Small Clusters**     | `bool`              | `false`      | Filter out clusters below minimum size                      |
| **Min Vtx Count**             | `int32`             | `3`          | Minimum vertices to keep cluster                            |
| **Min Edge Count**            | `int32`             | `3`          | Minimum edges to keep cluster                               |
| **Remove Big Clusters**       | `bool`              | `false`      | Filter out clusters above maximum size                      |
| **Max Vtx Count**             | `int32`             | `500`        | Maximum vertices to keep cluster                            |
| **Max Edge Count**            | `int32`             | `500`        | Maximum edges to keep cluster                               |
| **Refresh Edge Seed**         | `bool`              | `false`      | Regenerate random seeds for edges                           |
| **Build And Cache Clusters**  | `EPCGExOptionState` | `Default`    | Pre-build cluster topology for faster downstream processing |
| **Pre Build Face Enumerator** | `bool`              | `false`      | Pre-compute face/cell enumeration                           |
| **Output Edge Length**        | `bool`              | `false`      | Write edge length as attribute                              |
| **Edge Length Name**          | `FName`             | `EdgeLength` | Name for edge length attribute                              |

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin       | Type       | Description                                                 |
| --------- | ---------- | ----------------------------------------------------------- |
| **Vtx**   | Point Data | Cluster vertices (original points with cluster metadata)    |
| **Edges** | Point Data | Cluster edges with endpoint indices and optional attributes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Elements/PCGExConnectPoints.h)
