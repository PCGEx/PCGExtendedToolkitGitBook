---
description: >-
  A state provider that performs bulk directional adjacency checks and applies
  bitmask operations based on the results.
icon: circle-dashed
---

# State : Bitmask Adjacency

### Overview

This state provider checks whether a vertex's edges align with specified directions within an angular threshold. When edges match the directional criteria, bitmask compositions are applied to the vertex's flag attribute. This enables efficient bulk-checking of adjacency patterns using bitmask collections rather than individual filter nodes.

### How It Works

1. **Direction Extraction**: For each vertex, examines the directions to all connected neighbors.
2. **Angle Comparison**: Compares edge directions against the configured angle threshold.
3. **Bitmask Application**: If directions match (or don't match if inverted), applies the configured bitmask compositions.
4. **Filter Support**: Optionally uses filters to conditionally apply alternative bitmasks on failure.

**Usage Notes**

* **Bitmask Collections**: Use bitmask collections to efficiently encode multiple directional states.
* **Transform Direction**: Enable to transform direction checks into local point space.
* **Angle Threshold**: The angle determines how closely an edge must align to be considered a match.

### Settings

<details>

<summary><strong>Angle</strong> <code>double</code></summary>

The shared angle threshold (in degrees) for direction matching. Edges within this angle of a target direction are considered matches.

Default: `22.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform Direction</strong> <code>bool</code></summary>

Whether to transform direction checks using the vertex's point transform. Enable for local-space direction matching.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compositions</strong> <code>TArray&#x3C;FPCGExBitmaskRef></code></summary>

Bitmask operations executed on the flag if all filters pass (or if no filter is set).

</details>

<details>

<summary><strong>Collections</strong> <code>TMap&#x3C;UPCGExBitmaskCollection, EPCGExBitOp_OR></code></summary>

Bitmask collections and their associated operations to apply when conditions are met.

</details>

<details>

<summary><strong>Use Alternative Bitmasks On Filter Fail</strong> <code>bool</code></summary>

If enabled and filters exist, applies alternative bitmask operations when filters fail instead of the primary ones.

Default: `false`

</details>

<details>

<summary><strong>On Fail Compositions</strong> <code>TArray&#x3C;FPCGExBitmaskRef></code></summary>

Bitmask operations executed when filters fail.

📋 _Visible when Use Alternative Bitmasks On Filter Fail is enabled_

</details>

<details>

<summary><strong>On Fail Collections</strong> <code>TMap&#x3C;UPCGExBitmaskCollection, EPCGExBitOp_OR></code></summary>

Bitmask collections to apply when filters fail.

📋 _Visible when Use Alternative Bitmasks On Filter Fail is enabled_

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Whether to invert the dot product check. When enabled, bitmasks are applied when directions do NOT match.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/States/PCGExAdjacencyStates.h)
