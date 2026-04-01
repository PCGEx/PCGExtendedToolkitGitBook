---
description: Create points from staged data sockets.
icon: circle
---

# Staging : Load Sockets

### Overview

This node generates points from socket transforms defined in staged asset collection entries. Sockets are attachment points (like mesh sockets or custom collection sockets) that define specific locations and orientations on assets. The node transforms socket positions to world space based on staging data and outputs them as separate points with optional metadata attributes.

### How It Works

1. **Entry Retrieval**: Reads collection map references from staged points to identify source entries
2. **Socket Discovery**: Finds all sockets defined on each entry based on filtering criteria
3. **Transform Application**: Transforms socket local positions to world space using staging transforms
4. **Point Creation**: Creates output points at socket locations with configured attributes
5. **Attribute Writing**: Optionally writes socket metadata (name, tag, category, asset path) to points

**Usage Notes**

* **Staging Dependency**: Requires points to be staged first using Asset Staging node with Collection Map output mode
* **Socket Filtering**: Use tag and name filters to selectively output specific sockets
* **Transform Scale**: Controls which transform components (position, rotation, scale) are applied from staging
* **Carry Over**: Can transfer attributes from source points to socket points

### Inputs

| Pin    | Type       | Description                                  |
| ------ | ---------- | -------------------------------------------- |
| **In** | Point Data | Staged points with collection map references |

### Outputs

| Pin     | Type       | Description                        |
| ------- | ---------- | ---------------------------------- |
| **Out** | Point Data | Points created at socket locations |

### Settings

#### Socket Output Configuration

<details>

<summary><strong>Output Socket Details</strong> <code>FPCGExSocketOutputDetails</code></summary>

Comprehensive socket output configuration including:

**Socket Filtering:**

* **Socket Tag Filters** `FPCGExNameFiltersDetails`: Filter sockets by their tags (include/exclude patterns)
* **Socket Name Filters** `FPCGExNameFiltersDetails`: Filter sockets by their names (include/exclude patterns)

**Attribute Output:**

* **Write Socket Name** `bool`: Output socket name to an attribute
  * **Socket Name Attribute Name** `FName`: Name of the attribute to write socket name to (default: `SocketName`)
* **Write Socket Tag** `bool`: Output socket tag to an attribute
  * **Socket Tag Attribute Name** `FName`: Name of the attribute to write socket tag to (default: `SocketTag`)
* **Write Category** `bool`: Output entry category to an attribute
  * **Category Attribute Name** `FName`: Name of the attribute to write category to (default: `Category`)
* **Write Asset Path** `bool`: Output asset path to an attribute
  * **Asset Path Attribute Name** `FName`: Name of the attribute to write asset path to (default: `AssetPath`)

**Transform Configuration:**

* **Transform Scale** `uint8`: Bitmask controlling which transform components are applied (position, rotation, scale)

**Carry Over:**

* **Carry Over Settings** `FPCGExCarryOverDetails`: Controls which attributes are transferred from source points to socket points

Default: Basic socket output with no attribute writing

//→ See TODO FPCGExCarryOverDetails

</details>

#### Inherited Settings

This node inherits point filtering capabilities from its base class.

→ Point filters can be applied to control which staged points have their sockets processed

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingLoadSockets.h)
