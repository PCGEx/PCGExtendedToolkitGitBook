---
description: Load and spawn PCGDataAsset contents from staged points.
icon: circle
---

# Staging : Load PCGData

### Overview

This node loads and instantiates PCG Data Assets from points that have been staged using the Asset Staging node with Collection Map output. It transforms the data asset contents to match each point's position and rotation, applies tag filtering, forwards attributes, and routes output to custom pins based on data names.

### How It Works

1. **Entry Retrieval**: Reads collection map references from staged points to identify which data assets to load
2. **Asset Loading**: Loads unique data assets once (shared across all points using the same asset)
3. **Data Transformation**: Transforms spatial data (points, splines, surfaces, etc.) to match target point transforms
4. **Tag Filtering**: Optionally filters data by include/exclude tag sets
5. **Pin Routing**: Routes data to custom output pins by name, or to the default "Out" pin
6. **Attribute Forwarding**: Forwards specified attributes from source points to spawned data

**Usage Notes**

* **Staging Dependency**: Requires points to be staged first using Asset Staging node with Collection Map output mode
* **Shared Loading**: Data assets are loaded once and shared across all points referencing them for efficiency
* **Cluster ID Remapping**: Automatically remaps PCGEx cluster IDs to prevent conflicts when spawning multiple instances
* **Custom Pins**: Data matching custom pin names by exact match are routed to those pins, others go to default "Out"

### Inputs

| Pin    | Type       | Description                                  |
| ------ | ---------- | -------------------------------------------- |
| **In** | Point Data | Staged points with collection map references |

### Outputs

| Pin             | Type | Description                                      |
| --------------- | ---- | ------------------------------------------------ |
| **Out**         | Any  | Default output for data not matching custom pins |
| **Custom Pins** | Any  | User-defined pins for routing data by name       |

### Settings

#### Loading Configuration

<details>

<summary><strong>Custom Output Pins</strong> <code>TArray</code></summary>

Define custom output pins for routing data by name. Data from the PCG Data Asset will be routed to matching pins by exact name. Data that doesn't match any custom pin goes to the default "Out" pin.

Default: Empty (all data to "Out" pin)

</details>

<details>

<summary><strong>Refresh Seeds</strong> <code>bool</code></summary>

Regenerate random seeds on output points after loading.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Empty Data</strong> <code>bool</code></summary>

Filter out empty data sets from the output, even if they have potentially meaningful metadata attributes.

Default: `true`

⚡ PCG Overridable

</details>

#### Filtering

<details>

<summary><strong>Filter By Tags</strong> <code>bool</code></summary>

Enable tag-based filtering of data from the PCG Data Asset.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Include Tags</strong> <code>TSet</code></summary>

Tags to include. If empty, all data is included (unless excluded by Exclude Tags).

Default: Empty set (include all)

⚡ PCG Overridable

📋 _Visible when Filter By Tags is enabled_

</details>

<details>

<summary><strong>Exclude Tags</strong> <code>TSet</code></summary>

Tags to exclude. Data with any of these tags will be filtered out.

Default: Empty set

⚡ PCG Overridable

📋 _Visible when Filter By Tags is enabled_

</details>

#### Tagging & Forwarding

<details>

<summary><strong>Targets Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Controls which target attributes from source points are forwarded to spawned point data. Supports attribute filtering by name patterns.

Default: No forwarding

//→ See TODO FPCGExForwardDetails

</details>

<details>

<summary><strong>Forward Input Tags</strong> <code>bool</code></summary>

Forward tags from input point data to all spawned data.

Default: `true`

⚡ PCG Overridable

</details>

#### Warnings and Errors

<details>

<summary><strong>Quiet Unsupported Type Warnings</strong> <code>bool</code></summary>

Suppress warnings about unsupported spatial data types that cannot be transformed (e.g., certain volume or landscape data types).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Quiet Invalid Entry Warnings</strong> <code>bool</code></summary>

Suppress warnings about missing or invalid collection entries referenced by staged points.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits point filtering capabilities from its base class.

→ Point filters can be applied to control which staged points are processed

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingLoadPCGData.h)
