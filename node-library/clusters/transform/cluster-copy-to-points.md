---
description: Create copies of the input clusters onto the target points.
icon: share-nodes
---

# Cluster : Copy to Points

### Overview

This node duplicates cluster graphs at target point positions. Each target point receives a copy of the input cluster, transformed to match the target's location, rotation, and optionally scale. This enables instancing of graph structures across multiple positions with optional matching rules.

### How It Works

1. **Matching** (optional): Pairs input clusters with target points using attribute matching
2. **Transformation**: Copies clusters to each target point's transform
3. **Attribute Forwarding** (optional): Transfers target point attributes to cluster outputs
4. **Tagging** (optional): Converts target attributes to cluster tags

**Usage Notes**

* **No Sanitization**: Input clusters are copied as-is without validation. Ensure clean input data.
* **Data Matching**: Use matching to control which clusters go to which target points.
*   **Transform Inheritance**: Configure whether clusters inherit target rotation and scale.

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>#### Matching Clusters to Targets</p></div>

Enable **Data Matching** to pair specific clusters with specific target points using attribute values. Without matching, all clusters are copied to all targets.

### Inputs

| Pin         | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| **Vtx**     | Points | Cluster vertex points to copy               |
| **Edges**   | Points | Cluster edge data to copy                   |
| **Targets** | Points | Target points where clusters will be placed |

### Settings

#### Data Matching

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how clusters are matched to target points.

* **Mode**: Enable/disable matching
* **Cluster Match Mode**: Match on Vtx or Edge data
* **Split Unmatched**: Handle unmatched data separately
* **Limit Matches**: Restrict how many targets each cluster copies to

</details>

#### Transform

<details>

<summary><strong>Transform Details</strong> <code>FPCGExTransformDetails</code></summary>

Controls how clusters are transformed at each target point.

//→ See TODO FPCGExTransformDetails

</details>

#### Tagging & Forwarding

<details>

<summary><strong>Targets Attributes To Cluster Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Copy target point attribute values as tags on output clusters.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Targets Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Forward attributes from target points onto cluster outputs.

//→ See TODO FPCGExForwardDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Clusters Processor Settings for inherited options.

### Outputs

| Pin       | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| **Vtx**   | Points | Copied cluster vertices at target positions |
| **Edges** | Points | Copied cluster edges                        |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExCopyClustersToPoints.h)
