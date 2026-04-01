---
description: Output property values from staged entries as point attributes.
icon: circle
---

# Staging : Load Properties

### Overview

This node reads custom property values from staged asset collection entries and writes them to point attributes. Properties must be defined in the collection's property schema, and entries can provide overrides. This allows transferring collection-level metadata and entry-specific data to points for use in further processing or spawning logic.

### How It Works

1. **Entry Retrieval**: Reads collection map references from staged points to identify source entries
2. **Property Resolution**: Resolves properties for each unique entry, using entry overrides when available, otherwise collection defaults
3. **Cache Building**: Pre-computes property values for all unique entries to avoid per-point resolution overhead
4. **Attribute Writing**: Writes resolved property values to point attributes according to output configuration

**Usage Notes**

* **Staging Dependency**: Requires points to be staged first using Asset Staging node with Collection Map output mode
* **Property Schema**: Properties must be defined in the collection's property schema
* **Entry Overrides**: Individual entries can override collection-level property values
* **Efficient Caching**: Property values are resolved once per unique entry and cached for performance

### Inputs

| Pin    | Type       | Description                                  |
| ------ | ---------- | -------------------------------------------- |
| **In** | Point Data | Staged points with collection map references |

### Outputs

| Pin     | Type       | Description                                       |
| ------- | ---------- | ------------------------------------------------- |
| **Out** | Point Data | Points with property values written as attributes |

### Settings

#### Property Configuration

<details>

<summary><strong>Property Output Settings</strong> <code>FPCGExPropertyOutputSettings</code></summary>

Specifies which properties to read from collection entries and how to write them as point attributes. Contains an array of property output configurations, each defining:

* **Property Name**: The name of the property in the collection schema
* **Output Attribute Name**: The name of the attribute to write on points
* **Type Conversion**: Optional type conversion settings

Property names must match properties defined in the source collection. The node will use entry-specific overrides when available, otherwise it falls back to collection-level defaults.

Default: Empty (no properties output)

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits point filtering capabilities from its base class.

→ Point filters can be applied to control which staged points have properties written

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingLoadProperties.h)
