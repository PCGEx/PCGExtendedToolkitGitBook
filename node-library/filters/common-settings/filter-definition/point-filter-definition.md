---
description: Base factory data classes for all point filters.
icon: code
---

# Point Filter Definition

### Overview

This file defines the core filter factory data classes that form the foundation of the PCGExtended filtering system. Filter factory data objects are created by filter provider nodes and consumed by processing nodes to perform conditional operations on points. The hierarchy supports point-level filtering, cluster-aware filtering (through derived classes), and collection-level filtering.

### How It Works

1. **Factory Creation**: Filter provider nodes create factory data objects configured with user settings.
2. **Filter Instantiation**: When consumed, factories create filter instances (`IFilter`) for actual evaluation.
3. **Initialization**: Filters initialize against point data, setting up attribute readers and caches.
4. **Evaluation**: Filters test individual points (or elements) and return pass/fail results.
5. **Result Caching**: Results can be cached to avoid redundant evaluation during batch processing.

**Usage Notes**

* **Abstract Classes**: These base classes cannot be instantiated directly; use concrete filter implementations.
* **Collection Evaluation**: Some filters support evaluating entire collections rather than individual points.
* **Proxy Evaluation**: Specialized filters can evaluate proxy points for context-free testing.
* **Domain Checking**: Filters validate they're being used with compatible data types.

### Class Hierarchy

```
UPCGExFactoryData (base factory)
  └── UPCGExFilterFactoryData (all filters)
        └── UPCGExPointFilterFactoryData (point filters)
              ├── UPCGExFilterCollectionFactoryData (collection filters)
              └── UPCGExClusterFilterFactoryData (cluster filters - separate file)
```

### Data Types

| Data Type                   | Description              |
| --------------------------- | ------------------------ |
| **PCGEx \| Filter**         | Base filter type         |
| **PCGEx \| Filter (Point)** | Point-specific filters   |
| **PCGEx \| Filter (Data)**  | Collection-level filters |

### Filter Interfaces

The namespace `PCGExPointFilter` contains the runtime filter classes:

#### IFilter

Base interface for all filter implementations. Provides:

* Point-by-index testing
* Cluster node testing
* Edge testing
* Collection testing
* Result caching

#### ISimpleFilter

Simplified filter base for filters that only need basic point testing. Routes all test variants through the index-based test method.

#### ICollectionFilter

Specialized filter for collection-level evaluation. Tests entire point collections rather than individual points.

#### FManager

Filter manager that coordinates multiple filters during evaluation:

* Initializes filter stacks
* Caches combined results
* Supports parallel batch evaluation
* Handles both point and cluster element testing

### Factory Properties

All filter factory data classes share these internal properties:

| Property                      | Type                         | Description                                    |
| ----------------------------- | ---------------------------- | ---------------------------------------------- |
| `Priority`                    | `int32`                      | Evaluation order priority                      |
| `InitializationFailurePolicy` | `EPCGExFilterNoDataFallback` | How to handle init failures                    |
| `MissingDataPolicy`           | `EPCGExFilterNoDataFallback` | How to handle missing data                     |
| `bOnlyUseDataDomain`          | `bool`                       | Whether filter only uses data domain selectors |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Core/PCGExPointFilter.h)
