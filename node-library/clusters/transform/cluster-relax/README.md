---
description: Relax point positions using edges connecting them.
icon: share-nodes
---

# Cluster : Relax

### Overview

This node iteratively adjusts vertex positions to create a more evenly-distributed graph layout. Each iteration moves vertices based on their connected neighbors, gradually smoothing out the overall structure. The relaxation process can use different algorithms to determine how vertices are repositioned relative to their neighbors.

### How It Works

1. **Initialize Buffers**: Creates position buffers to track vertex movement during relaxation.
2. **Iterate Relaxation**: For each iteration, calculates new positions for all vertices based on their neighbors and the selected relaxation algorithm.
3. **Apply Influence**: Blends the relaxed position with the original based on the influence setting, which can be progressive (ramping up over iterations).
4. **Output Results**: Updates vertex positions and optionally writes movement direction and amplitude to attributes.

**Usage Notes**

* **Iteration Count**: More iterations produce smoother results but take longer to compute. Start with lower values and increase as needed.
* **Progressive Influence**: When enabled, influence ramps up over iterations, creating a gentler transition that preserves some original structure.
* **Algorithm Selection**: Different relaxation operations produce different results - Laplacian smoothing averages neighbor positions, while spring-based methods simulate physical forces.

### Settings

<details>

<summary><strong>Iterations</strong> <code>int32</code></summary>

Number of relaxation iterations to perform. More iterations produce a smoother, more evenly-distributed result.

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Relaxing</strong> <code>UPCGExRelaxClusterOperation</code></summary>

The relaxation algorithm to use. This is an instanced sub-node that determines how vertex positions are calculated from their neighbors.

Available operations include various smoothing and force-based algorithms.

</details>

#### Influence Settings

<details>

<summary><strong>Influence Input</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant influence value or read it from a point attribute.

| Option        | Description                                |
| ------------- | ------------------------------------------ |
| **Constant**  | Use a fixed influence value for all points |
| **Attribute** | Read influence per-point from an attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Influence</strong> <code>double</code></summary>

How much the relaxation affects vertex positions. A value of 1.0 fully applies the relaxed position, while lower values blend between original and relaxed positions.

Default: `1.0`

📋 _Visible when Influence Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Progressive Influence</strong> <code>bool</code></summary>

When enabled, the influence ramps up progressively over iterations rather than being applied at full strength from the start. This creates a smoother transition and preserves more of the original structure.

Default: `true`

⚡ PCG Overridable

</details>

#### Output Attributes

<details>

<summary><strong>Write Direction And Size</strong> <code>bool</code></summary>

Enable to write the final relaxation movement as a combined direction and magnitude vector.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction And Size Attribute</strong> <code>FName</code></summary>

Name of the FVector attribute to write the combined direction and size to.

Default: `"DirectionAndSize"`

📋 _Visible when Write Direction And Size = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Direction</strong> <code>bool</code></summary>

Enable to write the normalized direction of relaxation movement.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Attribute</strong> <code>FName</code></summary>

Name of the FVector attribute to write the direction to.

Default: `"Direction"`

📋 _Visible when Write Direction = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Amplitude</strong> <code>bool</code></summary>

Enable to write the magnitude of the relaxation movement (the length of the DirectionAndSize vector).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Amplitude Attribute</strong> <code>FName</code></summary>

Name of the double attribute to write the amplitude to.

Default: `"Amplitude"`

📋 _Visible when Write Amplitude = true_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common cluster processing settings from its base class.

> See Clusters Processor Settings for: Cluster handling, edge validation, and other common cluster configuration.

### Inputs

| Pin                      | Type       | Description                                               |
| ------------------------ | ---------- | --------------------------------------------------------- |
| **Vtx**                  | Points     | Cluster vertices to relax                                 |
| **Edges**                | Points     | Cluster edges defining connectivity                       |
| **Overrides : Relaxing** | Param Data | Optional parameter overrides for the relaxation operation |

### Outputs

| Pin       | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| **Vtx**   | Points | Relaxed cluster vertices with updated positions |
| **Edges** | Points | Cluster edges (unchanged)                       |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExRelaxClusters.h)
