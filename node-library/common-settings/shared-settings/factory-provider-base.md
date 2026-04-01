---
description: Abstract base class for nodes that create and output factory configurations.
icon: code
---

# Factory Provider (Base)

### Overview

UPCGExFactoryProviderSettings is the foundational base class for all factory provider nodes in PCGEx. Provider nodes are specialized param-generating nodes that create factory data objects containing operation configurations. These factories flow through the graph via param pins and are consumed by processing nodes to create runtime operations. The provider system enables modular, reusable operation definitions that can be configured once and applied across multiple processing contexts.

### Provider System

```
Factory Provider Pattern:

[Provider Node] → Creates factory data
      ↓ (param pin)
[Consumer Node] → Uses factory to create operations
      ↓
[Operations] → Execute data processing
```

### How It Works

1. **Factory Creation**: Provider node creates a factory data instance via `CreateFactory()`
2. **Configuration**: Factory data stores the provider's settings and configuration
3. **Output**: Factory data flows out through param pins to consumer nodes
4. **Priority Assignment**: Default priority can be set via `GetDefaultPriority()`
5. **Main Thread Execution**: Providers run on main thread for thread safety
6. **Param Output**: Factory data output as PCG param data type

**Usage Notes**

* **Abstract Base**: Cannot be instantiated directly - always use concrete provider implementations (Filter Provider, Heuristic Provider, etc.)
* **Main Thread Only**: Providers execute on the main thread (`CanExecuteOnlyOnMainThread` returns true)
* **Param Type**: Providers return `EPCGSettingsType::Param`, outputting configuration data rather than spatial data
* **Execution Ignored**: Provider execution policy is `Ignored` - they don't participate in standard execution ordering
* **Cache Invalidation**: Internal cache invalidator triggers re-creation when settings change

### Provider Types

Factory providers exist for different operation types:

**Filter Providers**:

* Create filter factory data
* Define filtering criteria and logic
* Examples: Numeric Filter Provider, Boolean Filter Provider

**Heuristic Providers**:

* Create heuristic factory data
* Define scoring/weighting algorithms
* Examples: Steepness Provider, Gradient Provider

**Noise Providers**:

* Create noise factory data
* Define noise generation algorithms
* Examples: Perlin Provider, Simplex Provider

**Picker Providers**:

* Create picker factory data
* Define value selection strategies
* Examples: Constant Picker Provider, Range Picker Provider

**Custom Providers**:

* User-defined factory types
* Application-specific operations

### Base Functionality

#### Factory Creation

**CreateFactory(Context, InFactory)**:

* Virtual method that derived classes override
* Creates and configures the factory data instance
* Can optionally reuse/update an existing factory
* Returns configured factory data ready for consumption

#### Priority System

**GetDefaultPriority()**:

* Returns default priority for this factory
* Can be overridden by derived classes
* Default: `0`
* Higher values execute first when multiple factories present

#### Output Configuration

**GetMainOutputPin()**:

* Returns the name of the primary output pin
* Derived classes specify their output pin name
* Default: Empty string (base implementation)

#### Cancellation Policy

**ShouldCancel(Context, Result)**:

* Determines if execution should stop based on preparation result
* Default: `true` (cancel on any preparation failure)
* Can be overridden for custom failure handling

### Context Structure

**FPCGExFactoryProviderContext**:

* Specialized context for provider execution
* Contains `OutFactory` pointer to created factory data
* Inherits from FPCGExContext for standard PCGEx functionality

### Element Implementation

**FPCGExFactoryProviderElement**:

* Execution element for provider nodes
* Runs on main thread only
* Disabled passthrough data handling
* Minimal logging in editor (returns false for `ShouldLog`)

### Inherited Settings

Providers inherit common settings from UPCGExSettings:

* Performance settings (Bulk Init Data, Cache Data, Scoped Attribute Get)
* Cleanup settings (Flatten Output, Cleanup Consumable Attributes)
* Error handling (Propagate Aborted Execution, Quiet warnings/errors)

### Related Systems

**Factory Data**:

* Created by providers
* Stores operation configuration
* See Factory Data

**Consumer Nodes**:

* Accept factory data from providers
* Create operations from factories
* Execute factory-defined logic

**Instanced Factories**:

* Alternative to provider-based factories
* Can be selected directly in node properties
* Providers offer more flexibility and reusability

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Factories/PCGExFactoryProvider.h)
