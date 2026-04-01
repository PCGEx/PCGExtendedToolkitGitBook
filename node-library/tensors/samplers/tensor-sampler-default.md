---
description: Samples a single location in the tensor field.
icon: function
---

# Tensor Sampler (Default)

### Overview

The default Tensor Sampler queries a tensor field at a single point and returns the combined directional influence. It provides basic sampling with configurable step size and error tolerance for adaptive sampling. This is the simplest sampler and serves as the base for more sophisticated sampling strategies.

### How It Works

1. **Probe Setup**: Takes a transform representing the sample location and orientation.
2. **Field Query**: Queries all active tensor operations at the probe position.
3. **Result Combination**: Combines tensor influences based on the sampling mode.
4. **Output**: Returns a tensor sample containing direction and magnitude.

**Usage Notes**

* **Base Sampler**: This is the default sampler used when no specific sampler is configured.
* **Step Fractions**: Control the adaptive step size range relative to the base radius.
* **Sub-Steps**: Allow finer sampling within a single query for improved accuracy.

### Settings

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base radius for tensor sampling operations. Affects the scale at which the field is queried.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Step Fraction</strong> <code>double</code></summary>

Minimum step size as a fraction of the base radius. Limits how small adaptive steps can become.

Default: `0.1`

Range: `0.01` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Step Fraction</strong> <code>double</code></summary>

Maximum step size as a fraction of the base radius. Limits how large adaptive steps can become.

Default: `1.0`

Range: `0.1` to `2.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Error Tolerance</strong> <code>double</code></summary>

Error tolerance for step size adaptation. Lower values produce more accurate results but may require more computation.

Default: `0.01`

Range: `0.001` to `0.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Sub Steps</strong> <code>int32</code></summary>

Maximum number of sub-steps per sample. Higher values improve accuracy in rapidly changing fields at the cost of performance.

Default: `4`

Range: `1` to `16`

⚡ PCG Overridable

</details>

#### Inherited Settings

This factory inherits from the instanced factory base.

→ See Instanced Factory for common factory options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Core/PCGExTensorSampler.h)
