---
description: Abstract base for all action factory types.
icon: arrow-down-from-arc
---

# Action

### Overview

This is the foundational base class for all action factories in the PCGEx Actions system. It provides the common interface and functionality that specific action types inherit, including filter integration and priority-based execution ordering. Actions are used within the Batch Actions node to perform conditional operations on points.

### How It Works

1. **Filter Integration**: Actions can have associated point filters that determine which points are affected
2. **Match Callbacks**: Provides OnMatchSuccess and OnMatchFail hooks for conditional logic
3. **Priority Ordering**: Actions are executed in priority order within the Batch Actions workflow
4. **Factory Pattern**: Creates operation instances that process points according to action-specific logic

**Usage Notes**

* **Abstract Base**: This class is not used directly - specific action types inherit from it
* **Filter Support**: All actions can optionally use point filters to determine affected points
* **Batch Integration**: Actions are designed for use within the Batch Actions node
* **Extensible**: New action types can be created by inheriting from this base

### Factory Properties

#### Filter Configuration

<details>

<summary><strong>Filter Factories</strong> <code>TArray></code></summary>

Array of point filter factories that determine which points this action affects. Filters are evaluated to produce match success/fail results that drive conditional action logic.

Default: Empty (no filtering)

📋 _Not overridable, configured by action implementations_

</details>

### Provider Settings

#### Execution Order

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Execution priority for action processing order within Batch Actions. Higher values are processed last, allowing control over the sequence of operations.

Default: `0`

⚡ PCG Overridable

📋 _Advanced_

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsActions-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsActions/Public/Core/PCGExActionFactoryProvider.h)
