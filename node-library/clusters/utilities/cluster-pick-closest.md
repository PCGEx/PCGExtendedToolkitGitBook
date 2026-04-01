---
description: Pick the clusters closest to input targets.
icon: share-nodes
---

# Cluster : Pick Closest

### Overview

This node selects clusters based on their proximity to target points. For each target, it finds the closest cluster and can keep, omit, or tag it accordingly. This enables spatial filtering of clusters based on external reference points.

### How It Works

1. **Target Processing**: Analyzes each target point's position
2. **Proximity Search**: Finds the closest cluster vertex or edge to each target
3. **Pick Resolution**: Handles duplicate picks based on the selected mode
4. **Action Application**: Keeps, omits, or tags clusters based on selection

**Usage Notes**

* **Search Mode**: Choose whether to measure distance to vertices or edge midpoints.
* **Duplicate Handling**: Use "Next Best" mode to ensure each target picks a unique cluster.
* **Bounds Expansion**: Expand search area if targets are outside cluster bounds.

### Inputs

| Pin         | Type   | Description                             |
| ----------- | ------ | --------------------------------------- |
| **Vtx**     | Points | Cluster vertex points                   |
| **Edges**   | Points | Cluster edge data                       |
| **Targets** | Points | Target points to measure proximity from |

### Settings

#### Search

<details>

<summary><strong>Search Mode</strong> <code>EPCGExClusterClosestSearchMode</code></summary>

What type of cluster element to measure distance to.

| Option           | Description                               |
| ---------------- | ----------------------------------------- |
| **Closest vtx**  | Measure distance to nearest vertex        |
| **Closest edge** | Measure distance to nearest edge midpoint |

Default: `Closest vtx`

</details>

<details>

<summary><strong>Pick Mode</strong> <code>EPCGExClusterClosestPickMode</code></summary>

How to handle when multiple targets would pick the same cluster.

| Option        | Description                                               |
| ------------- | --------------------------------------------------------- |
| **Only Best** | Allow duplicate picks (same cluster for multiple targets) |
| **Next Best** | If a cluster is already picked, choose the next closest   |

Default: `Only Best`

</details>

#### Action

<details>

<summary><strong>Action</strong> <code>EPCGExFilterDataAction</code></summary>

What to do with picked clusters.

| Option   | Description                                            |
| -------- | ------------------------------------------------------ |
| **Keep** | Output only the picked clusters                        |
| **Omit** | Output only the non-picked clusters                    |
| **Tag**  | Output all clusters with tags indicating picked status |

Default: `Keep`

</details>

<details>

<summary><strong>Keep Tag</strong> <code>FName</code></summary>

Tag to apply to kept/picked clusters.

_Visible when Action = Tag_

</details>

<details>

<summary><strong>Omit Tag</strong> <code>FName</code></summary>

Tag to apply to omitted/non-picked clusters.

_Visible when Action = Tag_

</details>

#### Search Bounds

<details>

<summary><strong>Target Bounds Expansion</strong> <code>double</code></summary>

Expand target bounds by this amount when searching for nearby clusters.

Default: `10`

</details>

<details>

<summary><strong>Expand Search Outside Target Bounds</strong> <code>bool</code></summary>

If no cluster is found within bounds, expand search to find the closest anywhere.

Default: `true`

</details>

#### Target Forwarding

<details>

<summary><strong>Target Attributes To Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copy target point attribute values as tags on the picked clusters.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Target Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Forward attributes from target points onto picked clusters.

//→ See TODO FPCGExForwardDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description               |
| --------- | ------ | ------------------------- |
| **Vtx**   | Points | Selected cluster vertices |
| **Edges** | Points | Selected cluster edges    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExPickClosestClusters.h)
