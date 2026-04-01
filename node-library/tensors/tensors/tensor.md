---
description: Base factory for tensor field definitions.
icon: arrow-down-from-arc
---

# Tensor

### Overview

Tensor Factory is the abstract base class for all tensor field providers in PCGEx. Tensors define directional influence fields that can guide point movement, pathfinding, or other spatial operations. Each tensor type (Pole, Flow, Spin, etc.) inherits from this base and implements specific field behavior.

### How It Works

1. **Factory Creation**: The provider settings create a factory data object containing the tensor configuration.
2. **Preparation**: The factory prepares internal data structures needed for field sampling.
3. **Operation Creation**: When consumed by a tensor-aware node, creates a tensor operation instance.
4. **Field Sampling**: The operation samples the tensor field at query positions to return directional influence.

**Usage Notes**

* **Abstract Base**: This class cannot be used directly — use specific tensor types like Tensor Pole, Tensor Flow, etc.
* **Priority**: When multiple tensors are combined, the Priority value determines evaluation order in ordered sampling modes.
* **Composition**: Multiple tensor factories can be connected to build complex composite fields.

### Inheritance

This is a base class. Specific tensor types include:

* **Tensor Pole** - Radial attraction/repulsion fields
* **Tensor Flow** - Directional flow fields from paths
* **Tensor Spin** - Rotational vortex fields
* **Tensor Noise** - Procedural noise-based fields
* **Tensor Surface** - Surface-aligned fields
* And more...

### Settings

#### Provider Settings

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Tensor evaluation priority when multiple tensors are combined. Only used when the sampler is in an ordered sampling mode. Higher priority tensors are evaluated first.

Default: `0`

⚡ PCG Overridable

</details>

#### Inherited Settings

Tensor factories inherit from the base factory provider.

→ See Factory Provider Settings for common factory options.

### Outputs

| Pin        | Type           | Description                                                  |
| ---------- | -------------- | ------------------------------------------------------------ |
| **Tensor** | Tensor Factory | Tensor field definition to be consumed by tensor-aware nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Core/PCGExTensorFactoryProvider.h)
