---
description: Sample a cluster vtx by using a stored Vtx ID.
icon: circle
---

# Sample : Vtx by ID

### Overview

This node looks up cluster vertices by their stored vertex ID and samples data from them. It reads a vertex ID attribute from input points, finds the corresponding vertex in the cluster data, and optionally applies the sampled transform or blends attribute values. This is useful for reconnecting points to their source vertices after processing.

### How It Works

1. **Read Vertex IDs**: Reads the vertex ID attribute from each input point.
2. **Lookup Vertices**: Finds the corresponding vertex in the cluster vertex data using the ID.
3. **Sample Data**: Retrieves transform and attribute data from the matched vertex.
4. **Apply Results**: Optionally applies the sampled transform directly and/or blends attribute values.

**Usage Notes**

* **Vertex ID Format**: The vertex ID is the first 32 bits of the PCGEx/VData attribute that clusters use for internal tracking.
* **Failed Samples**: Points that cannot find a matching vertex are marked as failed and can optionally be pruned from the output.
* **Filtering**: Point filters can be used to selectively process only certain points.

### Settings

<details>

<summary><strong>Vtx ID Source</strong> <code>FName</code></summary>

Name of the attribute that stores the vertex ID to look up. This should contain the first 32 bits of the PCGEx/VData.

Default: `"VtxId"`

</details>

#### Apply Sampling

<details>

<summary><strong>Apply Transform</strong> <code>bool</code></summary>

When enabled, applies the sampled vertex transform directly to the point. You can individually enable/disable position, rotation, and scale components.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Look At</strong> <code>bool</code></summary>

When enabled, orients the point to look at the sampled vertex position.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Align</strong> <code>EPCGExAxisAlign</code></summary>

The axis to align when applying the look-at transform.

| Option       | Description                               |
| ------------ | ----------------------------------------- |
| **Forward**  | Align the forward axis toward the target  |
| **Backward** | Align the backward axis toward the target |
| **Right**    | Align the right axis toward the target    |
| **Left**     | Align the left axis toward the target     |
| **Up**       | Align the up axis toward the target       |
| **Down**     | Align the down axis toward the target     |

Default: `Forward`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Up from...</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant up vector or read it from an attribute for the look-at calculation.

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Vector</strong> <code>FVector</code></summary>

The constant up vector to use for the look-at transform.

Default: `(0, 0, 1)` (World Up)

📋 _Visible when Use Up from... = Constant_

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Successes</strong> <code>bool</code></summary>

If enabled, add the specified tag to the output data if at least one point was successfully sampled.

Default: `false`

</details>

<details>

<summary><strong>Has Successes Tag</strong> <code>FString</code></summary>

The tag to add when at least one sample succeeds.

Default: `"HasSuccesses"`

📋 _Visible when Tag If Has Successes = true_

</details>

<details>

<summary><strong>Tag If Has No Successes</strong> <code>bool</code></summary>

If enabled, add the specified tag to the output data if no points were successfully sampled.

Default: `false`

</details>

<details>

<summary><strong>Has No Successes Tag</strong> <code>FString</code></summary>

The tag to add when no samples succeed.

Default: `"HasNoSuccesses"`

📋 _Visible when Tag If Has No Successes = true_

</details>

#### Advanced

<details>

<summary><strong>Process Filtered Out As Fails</strong> <code>bool</code></summary>

If enabled, points excluded by filters are marked as "failed". If disabled, they are skipped entirely, preserving any existing attribute values.

Default: `true`

</details>

<details>

<summary><strong>Prune Failed Samples</strong> <code>bool</code></summary>

If enabled, points that failed to sample anything will be removed from the output.

Default: `false`

</details>

#### Inherited Settings

This node inherits common point processing settings from its base class.

> See Points Processor Settings for: Point filters and other common configuration.

### Inputs

| Pin          | Type               | Description                                 |
| ------------ | ------------------ | ------------------------------------------- |
| **In**       | Points             | Points with vertex ID attributes to look up |
| **Vtx**      | Points             | Cluster vertices to sample from             |
| **Blenders** | Blend Op Factories | Optional attribute blending operations      |

### Outputs

| Pin     | Type   | Description                      |
| ------- | ------ | -------------------------------- |
| **Out** | Points | Points with sampled data applied |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExSampleVtxByID.h)
