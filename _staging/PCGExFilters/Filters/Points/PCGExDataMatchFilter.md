---
icon: filter
description: 'Filter : Data Match - Tests input data against targets using match rules.'
---

# Filter : Data Match

Tests input data against targets using match rules.

## Overview

Filter : Data Match evaluates each input data collection against a set of target collections using configurable match rules. It operates at the collection level — all points within a matching collection pass (or fail) together. Match rules are provided as sub-nodes and can be combined with All (every rule must pass) or Any (at least one rule must pass) logic. The result can be inverted.

## How It Works

1. **Load Targets**: Reads target data collections from the `Targets` input pin
2. **Initialize Match Rules**: Loads match rule factories from their input pins and builds a data matcher
3. **Evaluate Per Collection**: For each input collection, checks whether it matches any target collection according to the configured rules and mode
4. **Apply Result**: All points in a matching collection receive the same pass/fail result. Inversion flips the outcome.

#### Usage Notes

- **Collection-Level Filter**: This filter evaluates entire collections, not individual points. Every point in a collection gets the same result.
- **Match Rules Required**: Without match rule sub-nodes connected, the filter has no criteria to evaluate and will fall back to the missing data policy.
- **Not Cacheable**: Results depend on the target data provided at runtime, so this filter cannot be cached.

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **Targets** | Points | Target data collections to match against (required) |
| **Match Rules** | Factories | Match rule sub-nodes defining the matching criteria |

## Settings

### Node-Specific Settings

<details>
<summary><strong>Mode</strong> <code>EPCGExMapMatchMode</code></summary>

How match rules are combined when evaluating.

| Option | Description |
|--------|-------------|
| **All** | Every match rule must pass for the collection to match |
| **Any** | At least one match rule must pass for the collection to match |

Default: `All`

</details>

<details>
<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the filter result. When enabled, collections that match the rules will fail the filter, and collections that don't match will pass.

Default: `false`

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Filter Definition](../../../node-library/filters/common-settings/filter-definition/point-filter-definition.md) for shared filter options.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Filter** | Factory | The filter factory, to be connected to a consumer node's filter pin |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExDataMatchFilter.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 2 documented (Mode, bInvert via FPCGExDataMatchFilterConfig)
Inherited Properties: Referenced to UPCGExFilterProviderSettings
Inputs: Targets (Points), Match Rules (Factories)
Outputs: Filter (Factory)
Nested Types: FPCGExDataMatchFilterConfig (struct), EPCGExMapMatchMode (enum)
-->
