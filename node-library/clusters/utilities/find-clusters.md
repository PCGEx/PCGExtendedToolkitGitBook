---
description: Find vtx/edge pairs inside a soup of data collections.
icon: share-nodes
---

# Find Clusters

// TODO : Need more meat

### Overview

This node identifies and pairs vertex and edge point collections from a mixed collection of data. When you have multiple point collections and need to identify which ones form valid cluster pairs (matching Vtx and Edges), this node analyzes the cluster metadata to find and output properly paired data.

### How It Works

1. **Collection Analysis**: Examines all input point collections for cluster metadata
2. **Pair Matching**: Identifies Vtx collections that have matching Edge collections (and vice versa)
3. **Output Separation**: Outputs matched pairs on the appropriate pins

**Usage Notes**

* **Mixed Data Input**: Feed in collections where Vtx and Edge data is interleaved or unorganized.
* **Search Modes**: Choose whether to find all pairs, find Vtx based on Edge data, or find Edges based on Vtx data.
* **Validation**: Helps identify orphaned Vtx or Edge collections that lack their counterpart.

### Inputs

| Pin    | Type   | Description                               |
| ------ | ------ | ----------------------------------------- |
| **In** | Points | Mixed point collections to search through |

### Settings

<details>

<summary><strong>Search Mode</strong> <code>EPCGExClusterDataSearchMode</code></summary>

How to search for and pair cluster data.

| Option             | Description                                    |
| ------------------ | ---------------------------------------------- |
| **All**            | Find all valid Vtx/Edge pairs                  |
| **Vtx from Edges** | Given Edge data, find matching Vtx collections |
| **Edges from Vtx** | Given Vtx data, find matching Edge collections |

Default: `All`

</details>

<details>

<summary><strong>Skip Trivial Warnings</strong> <code>bool</code></summary>

Suppress warnings about minor input mismatches during triage.

Default: `false`

</details>

<details>

<summary><strong>Skip Important Warnings</strong> <code>bool</code></summary>

Suppress warnings that would also appear if using these inputs directly in cluster nodes.

Default: `false`

</details>

### Outputs

| Pin       | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| **Vtx**   | Points | Matched vertex point collections |
| **Edges** | Points | Matched edge point collections   |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExFindClustersData.h)
