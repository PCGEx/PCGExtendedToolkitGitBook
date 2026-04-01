---
icon: filter
description: 'Staging : Type Filter - Filters staged points by their collection entry type.'
---

# Staging : Type Filter

Filters staged points by their collection entry type.

## Overview

Staging : Type Filter separates points based on which collection type they were staged from. After asset staging assigns collection entries to points, this node reads the entry type (Mesh, Actor, Level, etc.) and filters or splits the point set accordingly. It operates in three modes: keep only matching types, remove matching types, or split into a separate output pin per type.

## How It Works

1. **Read Staging Data**: Reads the collection map from a connected staging node and the entry hash attribute from each point
2. **Resolve Type**: Looks up each point's staging hash to determine which collection type it came from
3. **Filter or Split**: Depending on the mode, keeps, removes, or routes points by type

#### Usage Notes

- **Requires Staging Data**: This node needs the `Labels` input pin connected to a staging node's collection map output. Without it, there's no way to resolve point types.
- **Type Hierarchy**: Type matching walks the parent chain — if a point's specific type isn't explicitly listed, its parent type is checked
- **Invalid Entries**: Points with no valid staging hash or unresolvable entries can be included or excluded via the `Include Invalid` setting

## Behavior

```
Include mode:     [All Points] → [Only Mesh + Actor points]  (others discarded)
Exclude mode:     [All Points] → [Everything except Level points]
Pin Per Type:     [All Points] → Pin "Mesh" | Pin "Actor" | Pin "Level" | Pin "Discarded"
```

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **In** | Points | Staged points to filter |
| **Labels** | Params | Collection map data from a staging node (required) |

## Settings

### Node-Specific Settings

<details>
<summary><strong>Filter Mode</strong> <code>EPCGExStagedTypeFilterMode</code></summary>

Controls how points are filtered based on their collection entry type.

| Option | Description |
|--------|-------------|
| **Include** | Keep only points that match the selected types |
| **Exclude** | Remove points that match the selected types |
| **Pin Per Type** | Split points into separate output pins, one per enabled type |

Default: `Include`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Type Config</strong> <code>FPCGExStagedTypeFilterDetails</code></summary>

Configures which collection types to match against. Contains:

- **Type Filter**: Map of collection type names to enabled/disabled state. Toggle each type on or off.
- **Include Invalid**: Whether points with no valid staging entry are included in the match result

Default: Empty (no types selected)

⚡ PCG Overridable

</details>

<details>
<summary><strong>Output Discarded</strong> <code>bool</code></summary>

If enabled, points that are filtered out are sent to a separate `Discarded` output pin instead of being dropped.

Default: `false`

⚡ PCG Overridable

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Points Processor Settings](../../node-library/common-settings/shared-settings/points-processor-settings.md) for shared processing options.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Out** | Points | Points that passed the filter (Include/Exclude modes) |
| **Discarded** | Points | Points that were filtered out (when Output Discarded is enabled) |
| **\<Type Name\>** | Points | One pin per enabled type (Pin Per Type mode only) |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingTypeFilter.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 3 documented (FilterMode, TypeConfig, bOutputDiscarded)
Inherited Properties: Referenced to UPCGExPointsProcessorSettings
Inputs: In (Points), Labels (Params)
Outputs: Out (Points), Discarded (Points), dynamic per-type pins
Nested Types: EPCGExStagedTypeFilterMode (enum), FPCGExStagedTypeFilterDetails (struct)
-->
