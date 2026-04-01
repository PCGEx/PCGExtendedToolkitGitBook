---
description: >-
  Abstract base factory that creates probe operations for finding nearby
  connections during graph construction.
icon: arrow-down-from-arc
---

# Probe Definition

### Overview

This is the abstract base class for all probe factories. Probe factories generate probe operations that determine how the Connect Points node discovers potential connections between points. Each probe type implements a different strategy for identifying which points should be evaluated as connection candidates.

### How It Works

1. **Factory Creation**: The node creates a probe factory data object when executed
2. **Operation Generation**: When consumed by Connect Points, the factory spawns a probe operation instance
3. **Connection Discovery**: The probe operation uses its specific algorithm to identify candidate connections for each point

**Usage Notes**

* **Abstract Base**: This class cannot be used directly. Use one of the concrete probe implementations (KNN, Anisotropic, Chain, etc.)
* **Multiple Probes**: Connect Points can accept multiple probe factories, combining their results to find all potential connections

### Inputs

| Pin   | Type | Description                                             |
| ----- | ---- | ------------------------------------------------------- |
| **-** | -    | No inputs - this is a self-contained factory definition |

### Settings

This is an abstract base class with no node-specific settings. All configuration is defined in the concrete probe implementations.

### Outputs

| Pin       | Type           | Description                                       |
| --------- | -------------- | ------------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory data consumed by Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Core/PCGExProbeFactoryProvider.h)
