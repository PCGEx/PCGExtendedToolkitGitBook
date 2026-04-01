---
description: Root base class for all point processing operations.
icon: code
---

# Points Processor Settings

### Overview

This is the foundational base class for all point processing operations in PCGExtendedToolkit. It provides the core infrastructure that all PCGEx nodes inherit, including batch processing support, point filter integration, input/output management, and multi-threading capabilities. This is an abstract base class with no user-facing settings of its own — all configuration comes from derived classes.

### How It Works

1. **Input Management**: Handles point data collections from input pins
2. **Filtering Support**: Provides infrastructure for point filter integration
3. **Batch Processing**: Coordinates multi-threaded point processing across datasets
4. **IO Management**: Manages point IO (Input/Output) collections and advancement
5. **Output Generation**: Stages processed point data to output pins
6. **Multi-threading**: Provides batch-based parallel processing infrastructure

This class serves as the foundation for three main processor families:

* **Points Processors**: Direct point operations (filtering, sampling, attribute manipulation)
* **Path Processors**: Ordered point sequences (paths, splines, trajectories)
* **Cluster Processors**: Graph structures (vertices and edges)

### Architecture

```
UPCGExPointsProcessorSettings (Root)
├── Path Processors
│   └── Operations on ordered point sequences
├── Cluster Processors
│   └── Operations on graph edges and vertices
└── Direct Point Processors
    └── Operations on unordered point collections
```

### Point Filters

Point processors can optionally support point filters, which allow selective processing based on:

* Attribute values
* Spatial conditions
* Tag matching
* Custom filter logic

Not all processors support filters — check individual processor documentation.

### Batch Processing

The base class provides sophisticated batch processing:

* **Multi-threading**: Parallel processing across multiple datasets
* **Batch Coordination**: Manages processing phases (initialization, work, writing)
* **Progress Tracking**: Advances through point collections systematically
* **Memory Management**: Efficient handling of large point datasets

### Input/Output Behavior

**Standard Pattern:**

* **Input**: Point data collections (can accept multiple datasets)
* **Output**: Processed point data collections
* **Transactional**: Some processors modify input data, others create new outputs

**Initialization Modes:**

* **Forward**: Pass input data through
* **NewOutput**: Create fresh output datasets
* **DuplicateInput**: Clone input before processing

Check individual processor settings for specific input/output behavior.

### Common Capabilities

All point processors inherit:

1. **Multiple Dataset Support**: Process multiple point collections in parallel
2. **Point Filter Integration**: Optional selective processing via filter factories
3. **Batch Multi-threading**: Efficient parallel processing
4. **Attribute Management**: Read and write point attributes
5. **IO Advancement**: Systematic iteration through point datasets
6. **Clean Passthrough**: Proper data forwarding when disabled

### Usage Pattern

This base class is never used directly. Instead, use derived processor types:

**For point operations**: Filtering, sampling, attribute manipulation, spatial queries **For paths**: Smoothing, resampling, tangent calculation, path analysis **For clusters**: Pathfinding, edge operations, graph analysis, topology modification

Each derived class adds specific functionality and user-facing settings.

### Inputs

| Pin                    | Type       | Description                                    |
| ---------------------- | ---------- | ---------------------------------------------- |
| **In** (varies)        | Point Data | Main point input (multiple datasets supported) |
| **Filters** (optional) | Filters    | Point filter factories (when supported)        |

### Outputs

| Pin              | Type       | Description          |
| ---------------- | ---------- | -------------------- |
| **Out** (varies) | Point Data | Processed point data |

_Actual pin names vary by derived processor type_

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Core/PCGExPointsProcessor.h)
