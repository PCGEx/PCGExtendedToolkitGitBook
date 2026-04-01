---
description: Base infrastructure for cluster/edge processing operations.
icon: code
---

# Cluster Processor Settings

### Overview

This is the foundational processor settings class for operations that work with graph clusters and edges in PCGExtendedToolkit. Clusters represent connected graph structures with vertices (points) and edges (connections). This base class provides the common infrastructure for all cluster-based operations, handling edge collections, cluster pairing, vertex-edge relationships, and batch processing coordination.

### How It Works

1. **Input Processing**: Accepts vertices and edges as separate input collections
2. **Cluster Pairing**: Matches edge datasets with their corresponding vertex datasets
3. **Cluster Building**: Constructs cluster representations from vertex-edge pairs
4. **Batch Management**: Organizes cluster processing into batches for multi-threading
5. **Edge Sorting**: Optionally supports sorting edges based on configurable rules
6. **Scoped Indexing**: Manages scoped attribute lookups for performance optimization
7. **Output Generation**: Produces processed vertices and edges as separate outputs

Derived classes implement specific cluster operations like pathfinding, edge filtering, cluster analysis, or topology modifications.

### Common Settings

All cluster processor types inherit these base settings:

<details>

<summary><strong>Scoped Index Lookup Build</strong> <code>EPCGExOptionState</code></summary>

Controls whether scoped attribute read is enabled. Disabling this on small datasets may greatly improve performance. Enabled by default for legacy reasons.

| Option       | Description                                       |
| ------------ | ------------------------------------------------- |
| **Default**  | Uses the global or parent default setting         |
| **Enabled**  | Builds scoped index lookups for attribute access  |
| **Disabled** | Skips scoped indexing (faster for small datasets) |

Scoped indexing allows efficient attribute lookups across vertex-edge relationships but adds overhead. For small datasets or operations that don't need cross-referencing, disabling can improve performance.

Default: `Default`

</details>

<details>

<summary><strong>Quiet Missing Cluster Pair Element</strong> <code>bool</code></summary>

Suppresses warnings when a cluster pair (vertices/edges) cannot be found or matched.

* **false** (default): Warnings are shown when edges lack matching vertices or vice versa
* **true**: Silently skips unmatched cluster pairs

Useful when intentionally processing partial or incomplete cluster data where mismatches are expected.

Default: `false`

</details>

### Input/Output Pattern

Cluster processors typically use:

**Inputs:**

* **Source Vertices**: Point data representing graph vertices
* **Source Edges**: Edge data defining connections between vertices

**Outputs:**

* **Output Vertices**: Processed vertex points
* **Output Edges**: Processed edge connections

The processor automatically pairs vertices with their corresponding edges based on metadata or tags.

### Edge Sorting

Some cluster processors support edge sorting, which can organize edges based on:

* Attribute values
* Directional properties
* Spatial relationships
* Custom sorting rules

Check individual processor documentation to see if edge sorting is supported or required.

### Usage Pattern

Cluster processors are used for:

1. **Pathfinding**: Finding routes through graph structures
2. **Edge Filtering**: Removing or selecting specific edges
3. **Cluster Analysis**: Computing cluster properties
4. **Topology Modification**: Adding, removing, or modifying connections
5. **Graph Operations**: Transformations on graph structures

Each derived processor specializes in a specific cluster operation type.

### Performance Considerations

* **Scoped Index Lookup**: Disable for small datasets to improve performance
* **Batch Processing**: Clusters are processed in batches for multi-threading efficiency
* **Edge Sorting**: May add overhead but enables order-dependent operations
* **Cluster Pairing**: Matching vertices to edges has some overhead; ensure proper metadata

### Inputs

| Pin                 | Type       | Description               |
| ------------------- | ---------- | ------------------------- |
| **Source Vertices** | Point Data | Graph vertices (points)   |
| **Source Edges**    | Point Data | Graph edges (connections) |

### Outputs

| Pin                 | Type       | Description        |
| ------------------- | ---------- | ------------------ |
| **Output Vertices** | Point Data | Processed vertices |
| **Output Edges**    | Point Data | Processed edges    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExGraphs-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExGraphs/Public/Core/PCGExClustersProcessor.h)
