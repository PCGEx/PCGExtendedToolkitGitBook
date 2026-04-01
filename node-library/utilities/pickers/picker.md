---
description: Core base classes for the picker system.
icon: arrow-down-from-arc
---

# Picker

### Overview

This is the foundational infrastructure for PCGEx pickers, which are used to select indices from collections (like points in a dataset). Pickers convert values into index selections, supporting both discrete (exact indices) and normalized (relative 0-1 values) modes. The base configuration provides common settings for index interpretation, truncation, and safety handling.

### Base Configuration

All picker types inherit these common settings from `FPCGExPickerConfigBase`:

#### Common Picker Settings

<details>

<summary><strong>Treat As Normalized</strong> <code>bool</code></summary>

Determines how picker values are interpreted.

* **False** (default): Values are treated as discrete indices (0, 1, 2, 3...). A value of 5 means "select index 5."
* **True**: Values are treated as normalized relative positions (0.0 to 1.0). A value of 0.5 means "select the item at 50% through the collection."

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Truncate Mode</strong> <code>EPCGExTruncateMode</code></summary>

When using normalized values, determines how fractional positions are converted to discrete indices.

| Option    | Description                                |
| --------- | ------------------------------------------ |
| **Round** | Rounds to nearest index (0.4 → 0, 0.6 → 1) |
| **Ceil**  | Rounds up (0.1 → 1, 0.9 → 1)               |
| **Floor** | Rounds down (0.1 → 0, 0.9 → 0)             |

Default: `Round`

📋 _Visible when Treat As Normalized is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Safety</strong> <code>EPCGExIndexSafety</code></summary>

Determines how to handle out-of-bounds index picks (indices less than 0 or greater than the collection size).

| Option     | Description                                              |
| ---------- | -------------------------------------------------------- |
| **Ignore** | Skip invalid indices silently                            |
| **Clamp**  | Clamp to valid range (negative → 0, too large → max)     |
| **Wrap**   | Wrap around using modulo (treats collection as circular) |
| **Tile**   | Ping-pong/mirror at boundaries                           |

Default: `Ignore`

⚡ PCG Overridable

</details>

### Factory Types

The picker system uses a factory pattern:

* **UPCGExPickerFactoryData**: Base factory class that stores and processes picks
  * Maintains `DiscretePicks` array for index-based selections
  * Maintains `RelativePicks` array for normalized selections
  * Provides `AddPicks()` method to populate pick sets
* **UPCGExPickerFactoryProviderSettings**: Base provider class that creates picker factories
  * Outputs to the `Picker` pin
  * Derived classes implement specific picker behaviors

### Picker Categories

Pickers are used throughout PCGEx for index-based selection tasks:

* **Attribute Set Pickers**: Select indices based on attribute values
* **Constant Pickers**: Use fixed values for picks
* **Range Pickers**: Generate picks within specified ranges

Each picker type extends these base classes and adds specific functionality while inheriting the common configuration settings above.

### Usage Pattern

```
Picker Provider Node
      ↓
Creates Factory (UPCGExPickerFactoryData)
      ↓
Processes Input (attributes, constants, etc.)
      ↓
Generates Picks (discrete or normalized)
      ↓
Output: Set of indices for selection
```

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExPickers-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExPickers/Public/Core/PCGExPickerFactoryProvider.h)
