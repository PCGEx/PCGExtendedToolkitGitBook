---
description: Partition clusters by binning a numeric attribute.
icon: function
---

# Decomp : Threshold

### Overview

Reads a numeric attribute from each cluster node and assigns cell IDs based on value ranges. Supports two binning strategies: Uniform (equal-width bins across the value range) and Quantile (equal-count bins where each cell gets roughly the same number of nodes). This is a purely attribute-driven decomposition with no spatial or topological awareness.

### How It Works

1. **Attribute Read**: Reads the selected numeric attribute from each cluster node.
2. **Binning**:
   * **Uniform**: Divides the attribute's value range into equal-width bins. Each node is assigned to the bin its value falls into.
   * **Quantile**: Sorts nodes by attribute value and divides them into equal-count groups.
3. **Cell Assignment**: Each bin becomes a cell ID. The actual cell count may be less than Num Bins if many nodes share identical values.

### Settings

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The numeric attribute to read values from. Must be a numeric type (int32, float, double).

</details>

<details>

<summary><strong>Num Bins</strong> <code>int32</code></summary>

Number of bins to create.

Default: `4`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Binning Mode</strong> <code>EPCGExDecompBinningMode</code></summary>

| Option       | Description                                                      |
| ------------ | ---------------------------------------------------------------- |
| **Uniform**  | Equal-width bins across the value range                          |
| **Quantile** | Equal-count bins (each bin has roughly the same number of nodes) |

Default: `Uniform`

⚡ PCG Overridable

</details>

#### Inherited Settings

This is a decomposition sub-node consumed by [.](./ "mention").

→ See [decomposition-factory.md](decomposition-factory.md "mention") for the base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClustersDecomp-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersDecomp/Public/Decompositions/PCGExDecompThreshold.h)
