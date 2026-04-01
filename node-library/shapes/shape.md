---
description: Creates a single shape builder node, to be used with a Shape processor node.
icon: arrow-down-from-arc
---

# Shape

### Overview

Shape Builder factories define how points are generated in specific patterns. This is the base system that concrete shape types (Circle, Grid, Polygon, Fiblat) extend to provide their specific point generation algorithms. Shape builders are consumed by the Create Shapes node to generate point collections at each source point's location.

### Architecture

The shape builder system uses a factory/provider pattern:

* **Factory** (`UPCGExShapeBuilderFactoryData`): Abstract base that creates shape builder operations
* **Provider** (`UPCGExShapeBuilderFactoryProviderSettings`): Abstract base for shape builder definition nodes

Concrete implementations include:

* **Shape : Circle** - Generates points in circular patterns
* **Shape : Grid** - Generates points in grid layouts
* **Shape : Polygon** - Generates points along polygon edges
* **Shape : Fiblat** - Generates points using Fibonacci lattice distribution

### Behavior

**Shape Builder Flow:**

```
Shape Builder Definition (e.g., Circle, Grid)
         ↓
    Factory Output
         ↓
    Create Shapes Node
         ↓
    Points at each source location
```

### Usage

Shape builders are created through their specific definition nodes (e.g., "Shape : Circle") and connected to a Create Shapes or Shape Processor node. The shape builder defines the point pattern, while the processing node handles placement and transformation.

### Output Type

| Type      | Description                                              |
| --------- | -------------------------------------------------------- |
| **Shape** | Shape builder factory data for use with shape processors |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsShapes-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsShapes/Public/Core/PCGExShapeBuilderFactoryProvider.h)
