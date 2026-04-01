---
description: >-
  Base factory class for creating blend operations that define how attributes
  are blended between data sources.
icon: code
---

# Blend Op Factory

### Overview

This factory creates blend operation instances that handle attribute blending between two operands (A and B). Each operation can read from different data sources, apply weighted blending based on various modes, and output results to new or existing attributes. The factory handles preparation of constant data facades and manages dependencies for the blending operation.

### How It Works

1. **Factory Registration**: The factory registers itself with the PCGEx factory system as a Blending type operation.
2. **Preparation Phase**: During preparation, it resolves constant operand data if needed and registers asset dependencies (such as weight curves).
3. **Operation Creation**: Creates FPCGExBlendOperation instances configured with the blend settings.
4. **Buffer Registration**: Identifies which attributes need to be loaded based on the blend configuration (operand selectors, weight attributes).

**Usage Notes**

* **Factory Pattern**: This is a factory data class used internally by the PCGEx blending system. Users typically interact with blend operations through provider nodes rather than directly with factory instances.
* **Constant Facades**: The factory can create lightweight constant data facades for operands that use inline values rather than attributes.
* **Dependency Management**: Automatically tracks curve assets and attribute dependencies to ensure proper loading order.

### Behavior

```
Factory Creation Flow:
┌──────────────────┐
│ Blend Provider   │
│ Node             │
└────────┬─────────┘
         │ creates
         ▼
┌──────────────────┐
│ Blend Op Factory │
└────────┬─────────┘
         │ PrepareForData
         ▼
┌──────────────────┐
│ Blend Operation  │ ─► Performs blending
│ Instance         │    on actual data
└──────────────────┘
```

### Settings

#### Node-Specific Settings

This factory class has no directly editable properties. Configuration is handled through:

* Blend operation instances created by this factory
* Provider nodes that instantiate the factory
* Inherited factory data settings

#### Inherited Settings

This factory inherits settings from its base class.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/Core/PCGExBlendOpFactory.h)
