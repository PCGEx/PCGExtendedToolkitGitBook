---
description: >-
  Configures which attributes and tags are carried over when creating or
  transforming point data.
icon: sliders-simple
---

# Carry Over Details

### Overview

This settings block controls metadata inheritance during operations that create new points or merge existing ones. When points are fused, merged, or transformed into new data structures, these settings determine which attributes and tags from the source data are preserved in the output. Separate filtering rules can be applied to attributes and tags independently.

### How It Works

1. **Process Attributes**: Apply attribute name filters to determine which attributes transfer
2. **Process Tags**: Apply tag name filters to determine which tags transfer
3. **Transfer Data**: Matching attributes and tags are copied to the resulting points

**Usage Notes**

* **Data Domain**: When enabled, data-level attributes (collection metadata) are converted to point-level attributes on the output.
* **Tag Flattening**: When enabled, tag values are simplified during transfer rather than preserving their full structure.

### Behavior

```
Source Point Data:
  Attributes: [Position] [Color] [temp_id] [Scale]
  Tags: ["Building", "LOD:High", "debug_marker"]

Attribute Filter: Exclude "temp" (StartsWith)
Tag Filter: Exclude "debug" (StartsWith)

Output Point Data:
  Attributes: [Position] [Color] [Scale]
  Tags: ["Building", "LOD:High"]
```

### Settings

<details>

<summary><strong>Preserve Attributes Default Value</strong> <code>bool</code></summary>

When enabled, attributes retain their default values during carry-over even if no source data matches.

Default: `false`

</details>

<details>

<summary><strong>Attributes</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filters which attributes are carried over to the output data. Uses name-based pattern matching to include or exclude specific attributes.

→ See Name Filters Details for filter options

⚡ PCG Overridable

</details>

<details>

<summary><strong>Data Domain to Elements</strong> <code>bool</code></summary>

When enabled, attributes stored at the data/collection level are converted to per-point attributes on the output elements.

Default: `true`

</details>

<details>

<summary><strong>Tags</strong> <code>FPCGExNameFiltersDetails</code></summary>

Filters which tags are carried over to the output data. Uses name-based pattern matching to include or exclude specific tags.

→ See Name Filters Details for filter options

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flatten Tag Value</strong> <code>bool</code></summary>

When enabled, tag values are flattened during transfer, simplifying complex tag structures.

Default: `false`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Data/Utils/PCGExDataFilterDetails.h)
