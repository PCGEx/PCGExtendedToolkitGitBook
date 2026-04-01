---
description: Base class for all filter factory provider nodes.
icon: arrow-down-from-arc
---

# Filter Definition

### Overview

This is the abstract base class that defines the common structure and settings for all filter provider nodes in PCGExtended. Filters are conditional tests that evaluate points or elements and return pass/fail results. Each concrete filter type inherits the settings documented here and adds its own specific configuration. Filter providers create factory objects that are consumed by processing nodes to perform conditional operations.

### How It Works

1. **Configuration**: Filter providers are created as nodes that output filter factory definitions.
2. **Priority Ordering**: When multiple filters are used together, Priority determines evaluation order.
3. **Initialization**: Filters initialize against the target data, validating required attributes exist.
4. **Failure Handling**: If initialization fails (e.g., missing attributes), the failure policy determines behavior.
5. **Consumer Integration**: Processing nodes receive filter factories and apply them during execution.

**Usage Notes**

* **Abstract Base**: This class cannot be instantiated directly; use concrete filter implementations.
* **Failure Policies**: Choose Error to halt on problems, Pass/Fail to continue with a default result.
* **Priority**: Lower values are evaluated first when order matters for filter combinations.

### Base Settings

All filter nodes inherit these settings unless otherwise noted.

#### Core Settings

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Filter evaluation priority. When multiple filters are combined, lower priority values are evaluated first. Useful when filter order affects results or performance.

Default: `0`

⚡ PCG Overridable

</details>

#### Error Handling

<details>

<summary><strong>Initialization Failure Policy</strong> <code>EPCGExFilterNoDataFallback</code></summary>

How to handle failed filter initialization. Usually caused by missing attributes or unsupported filter configurations.

| Option    | Description                              |
| --------- | ---------------------------------------- |
| **Error** | Throw an error and halt execution        |
| **Pass**  | Treat as if all elements pass the filter |
| **Fail**  | Treat as if all elements fail the filter |

Default: `Error`

</details>

<details>

<summary><strong>Missing Data Policy</strong> <code>EPCGExFilterNoDataFallback</code></summary>

How to handle missing data during filter evaluation. Only applies to filters that rely on additional data pins.

| Option    | Description                                    |
| --------- | ---------------------------------------------- |
| **Error** | Throw an error and halt execution              |
| **Pass**  | Treat as if elements pass when data is missing |
| **Fail**  | Treat as if elements fail when data is missing |

Default: `Fail`

📋 _Visible only for filters that use external data inputs_

</details>

### Filter Types

{% content-ref url="point-filter-definition.md" %}
[point-filter-definition.md](point-filter-definition.md)
{% endcontent-ref %}

{% content-ref url="cluster-filter-definition.md" %}
[cluster-filter-definition.md](cluster-filter-definition.md)
{% endcontent-ref %}

#### Filter Collections

Grouped filters that combine multiple filter results.

| Data Type                        | Description                                 |
| -------------------------------- | ------------------------------------------- |
| **PCGEx \| Filter (Collection)** | Collections of filters evaluated as a group |

**Provider Settings**: `UPCGExFilterCollectionProviderSettings`

### Outputs

| Pin        | Type                    | Description                                                       |
| ---------- | ----------------------- | ----------------------------------------------------------------- |
| **Filter** | PCGEx \| Filter (Point) | Outputs the configured filter factory for use in processing nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Core/PCGExFilterFactoryProvider.h)
