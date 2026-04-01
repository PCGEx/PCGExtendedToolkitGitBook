---
description: >-
  Common settings inherited by most PCGEx nodes for performance, cleanup, and
  error handling.
icon: arrow-down-from-arc
---

# Shared Settings

### Overview

This is the base settings class that provides common functionality across the PCGExtendedToolkit. Most PCGEx nodes inherit these settings, which control performance optimizations (caching, bulk initialization, attribute scoping), output cleanup (flattening, consumable attribute removal), and error reporting behavior. Understanding these settings helps optimize graph performance and manage data flow consistently.

### Settings Categories

#### Performance

<details>

<summary><strong>Bulk Init Data</strong> <code>EPCGExOptionState</code></summary>

Pre-allocates all data on a single thread to avoid contention during multi-threaded processing.

| Option       | Description                   |
| ------------ | ----------------------------- |
| **Default**  | Use node's default behavior   |
| **Enabled**  | Force bulk initialization on  |
| **Disabled** | Force bulk initialization off |

Not all nodes support this optimization. When supported, it can improve performance by reducing lock contention.

Default: `Default`

</details>

<details>

<summary><strong>Cache Data</strong> <code>EPCGExOptionState</code></summary>

Caches node results to avoid re-computation when inputs remain unchanged.

| Option       | Description                       |
| ------------ | --------------------------------- |
| **Default**  | Use node's default caching policy |
| **Enabled**  | Force caching on                  |
| **Disabled** | Force caching off                 |

Cached data persists until inputs change. Useful for expensive operations that don't need to run every frame.

Default: `Default`

</details>

<details>

<summary><strong>Scoped Attribute Get</strong> <code>EPCGExOptionState</code></summary>

Controls whether attribute reads use scoping (safe but slower) or direct access (fast but less safe).

| Option       | Description                                      |
| ------------ | ------------------------------------------------ |
| **Default**  | Use node's default scoping policy                |
| **Enabled**  | Force scoped attribute reads                     |
| **Disabled** | Disable scoped reads (faster for small datasets) |

Disabling on small datasets may greatly improve performance. Enabled by default for legacy compatibility.

Default: `Default`

</details>

<details>

<summary><strong>Steal Data</strong> <code>EPCGExOptionState</code></summary>

Modifies input data directly instead of making copies (zero-copy optimization).

| Option                 | Description                        |
| ---------------------- | ---------------------------------- |
| **Default**            | Copy input data (safe)             |
| **Enabled**            | Modify inputs directly (dangerous) |
| **Disabled** (default) | Never steal data                   |

{% hint style="danger" %}
## **DANGER**

Only enable if you're absolutely certain the input data isn't used by any other node. This can corrupt data if misused and cause crashes.
{% endhint %}

📋 _Visible when node supports data stealing_

Default: `Disabled`

</details>

<details>

<summary><strong>Execution Policy</strong> <code>EPCGExExecutionPolicy</code></summary>

Forces execution over a single frame instead of spreading across multiple frames.

Not safe on all nodes - some nodes override this internally.

**WARNING**: Only change this if you know what you're doing.

📋 _Visible when Execution Policy != Ignored_

Default: `Default`

</details>

#### Cleanup

<details>

<summary><strong>Flatten Output</strong> <code>bool</code></summary>

Merges hierarchical data into a single flat collection and flatten all metadata inheritance.

Default: `false`

</details>

<details>

<summary><strong>Cleanup Consumable Attributes</strong> <code>bool</code></summary>

Deletes attributes marked as "consumable" from the output data.

Consumable attributes are temporary working data that don't need to persist. Cleaning them up reduces memory usage and clutter.

{% hint style="info" %}
Most per-point attribute selectors used for read operation are automatically declared as "consumables".
{% endhint %}

Default: `false`

</details>

<details>

<summary><strong>Protected Attributes (String)</strong> <code>FString</code></summary>

Comma-separated list of attribute names to exclude from cleanup.

Even if an attribute is marked consumable, listing it here prevents deletion.

Examples:

* `"TempIndex,DebugValue"`: Protects TempIndex and DebugValue
* `"Iterations"`: Protects only Iterations

📋 _Visible when Cleanup Consumable Attributes = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Protected Attributes (Array)</strong> <code>TArray</code></summary>

Array-based attribute protection (works alongside the string list above).

Provides a more structured way to specify protected attributes.

📋 _Visible when Cleanup Consumable Attributes = true_

</details>

#### Warnings and Errors

<details>

<summary><strong>Propagate Aborted Execution</strong> <code>bool</code></summary>

Cancels the entire graph execution if this node aborts internally.

When enabled, node failure stops the whole graph. When disabled, only this node stops.

Default: `false`

</details>

<details>

<summary><strong>Quiet Invalid Input Warning</strong> <code>bool</code></summary>

Suppresses warnings about invalid input data (missing attributes, wrong data types, etc.).

Useful when invalid inputs are expected and should be silently ignored.

Default: `false`

</details>

<details>

<summary><strong>Quiet Missing Input Error</strong> <code>bool</code></summary>

Suppresses errors about missing required input connections.

Use when optional inputs are designed to be sometimes absent.

Default: `false`

</details>

<details>

<summary><strong>Quiet Cancellation Error</strong> <code>bool</code></summary>

Suppresses error messages when node execution is cancelled.

Useful when cancellation is a normal part of the workflow.

Default: `false`

</details>

### Usage Notes

**Option State Pattern**: Many performance settings use EPCGExOptionState (Default, Enabled, Disabled), allowing per-node override of global defaults.

**Data Stealing**: The Steal Data option is extremely dangerous - only use it when you have complete control over the data flow and know the input isn't used elsewhere.

**Consumable Attributes**: When cleaning up consumables, use Protected Attributes to preserve any working data you need to keep for debugging or downstream processing.

**Error Suppression**: Quieting errors can hide real problems - only suppress when you have a specific reason and understand the implications.

**Caching**: Enabling Cache Data can dramatically speed up iterative workflows where inputs change infrequently.

### Inherited By

This base class is inherited by:

* **UPCGExPointsProcessorSettings** - Point data processing nodes
* **UPCGExClustersProcessorSettings** - Cluster processing nodes
* **UPCGExPathProcessorSettings** - Path processing nodes
* Many other specialized base classes

Most user-facing PCGEx nodes inherit these settings through intermediate base classes.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Core/PCGExSettings.h)
