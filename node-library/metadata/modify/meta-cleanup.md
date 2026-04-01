---
description: Keep/Remove tags & attributes using string queries.
icon: circle
---

# Meta Cleanup

### Overview

This node filters metadata from point collections based on string matching rules. It can selectively keep or remove attributes and tags that match specified patterns. The filtering supports various match modes including exact match, contains, starts with, and ends with.

### How It Works

1. **Parse Filters**: Reads attribute and tag filter configurations.
2. **Match Names**: Evaluates each attribute and tag name against the filter patterns.
3. **Apply Action**: Keeps or removes matching metadata based on filter mode.
4. **Output**: Returns cleaned point data with filtered metadata.

**Usage Notes**

* **Filter Modes**: Choose between keeping only matching items or removing matching items.
* **Match Modes**: Use exact match for specific names, or pattern matching for broader cleanup.
* **Comma Separated**: Specify multiple filter patterns in a comma-separated list.
* **Preserve Defaults**: Option to preserve attribute default values during cleanup.

### Behavior

```
Metadata Cleanup:

Input Point Data:
   Attributes: ["PCGEx_TempValue", "BiomeType", "PCGEx_Debug", "Health"]
   Tags: ["PCGEx:Generated", "Level:1", "Spawner:Trees"]

Filter: Remove attributes starting with "PCGEx_"
Filter: Remove tags starting with "PCGEx:"

Output:
   Attributes: ["BiomeType", "Health"]
   Tags: ["Level:1", "Spawner:Trees"]
```

### Inputs

| Pin    | Type   | Description                      |
| ------ | ------ | -------------------------------- |
| **In** | Points | Input point collections to clean |

### Settings

#### Attribute Filters

<details>

<summary><strong>Attributes</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filters for attribute cleanup.

//→ See TODO FPCGExNameFiltersDetails

</details>

<details>

<summary><strong>Data Domain to Elements</strong> <code>bool</code></summary>

When enabled, applies @Data domain attribute filters to element-level attributes as well.

Default: `true`

</details>

<details>

<summary><strong>Preserve Attributes Default Value</strong> <code>bool</code></summary>

When enabled, preserves the default value of attributes during cleanup operations.

Default: `false`

</details>

#### Tag Filters

<details>

<summary><strong>Tags</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filters for tag cleanup.

| Property                  | Type   | Description                                |
| ------------------------- | ------ | ------------------------------------------ |
| **Filter Mode**           | enum   | All, Include matching, or Exclude matching |
| **Comma Separated Names** | string | Tag names or patterns to match             |
| **Match Mode**            | enum   | Equals, Contains, StartsWith, EndsWith     |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flatten Tag Value</strong> <code>bool</code></summary>

When enabled, flattens tag values during filtering.

Default: `false`

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| **Out** | Points | Points with cleaned attributes and tags |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExMetaCleanup.h)
