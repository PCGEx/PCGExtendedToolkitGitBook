---
description: Probe using bitmask references and collections.
icon: circle-dashed
---

# Probe : Bitmasks

### Overview

This per-point probe uses bitmask definitions to control directional connectivity. Each bitmask defines a direction and matching rules, allowing complex connection patterns based on bit operations. Points connect to neighbors whose direction matches the bitmask criteria, with prioritization based on either alignment quality or distance.

### How It Works

1. **Bitmask Loading**: Loads bitmask compositions and collections that define probe directions
2. **Direction Setup**: Extracts direction vectors from bitmask definitions
3. **Candidate Search**: For each point, finds candidates within the search radius
4. **Mask Matching**: Evaluates candidates against bitmask rules and angle tolerance
5. **Prioritization**: Selects best matches based on alignment or distance preference

**Usage Notes**

* **Bitmask Collections**: Define reusable sets of directions via UPCGExBitmaskCollection assets
* **Composition Rules**: Combine multiple bitmask operations using bit logic (OR, AND, etc.)
* **Direction Transform**: When enabled, bitmask directions rotate with each point's orientation

### Behavior

```
Bitmask-Directed Probing:

    Bitmask defines connection directions
    with specific angle tolerances:

         ╲  2  ╱
          ╲   ╱
       1   ╲ ╱   3
            ●
       6   ╱ ╲   4
          ╱   ╲
         ╱  5  ╲

    Each numbered zone is defined by a bitmask entry.
    Connections only made to neighbors matching
    the bitmask criteria within angle tolerance.


    Favor modes:

    Best Alignment:              Closest Position:
        ╲                            ╲
         ╲                            •(near but misaligned)
          ╲ • (best aligned)           ╲
           ╲│                           ╲│
            ●                            ●
              connects to                 connects to nearest
              best-aligned                regardless of alignment
```

### Settings

#### Bitmask Configuration

<details>

<summary><strong>Transform Direction</strong> <code>bool</code></summary>

When enabled, rotates the bitmask-defined directions according to each point's transform. This allows the connection pattern to follow the point's orientation.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Favor</strong> <code>EPCGExProbeBitmasksPriorization</code></summary>

Determines how to prioritize among candidates that match the bitmask criteria.

| Option               | Description                                                                       |
| -------------------- | --------------------------------------------------------------------------------- |
| **Best Alignment**   | Favor candidates that best align with the bitmask direction, even if farther away |
| **Closest Position** | Favor the closest candidates within the angle tolerance                           |

Default: `Closest Position`

</details>

<details>

<summary><strong>Angle</strong> <code>double</code></summary>

The shared angle threshold for all bitmask directions. Candidates must be within this angle of a bitmask direction to be considered.

Default: `22.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compositions</strong> <code>TArray&#x3C;FPCGExBitmaskRef></code></summary>

Individual bitmask references defining probe directions and their matching rules.

</details>

<details>

<summary><strong>Collections</strong> <code>TMap&#x3C;UPCGExBitmaskCollection, EPCGExBitOp_OR></code></summary>

Bitmask collection assets to include, with OR operations applied to combine their entries.

</details>

#### Search Radius (Inherited)

<details>

<summary><strong>Search Radius Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the search radius is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the radius from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Search Radius</strong> <code>double</code></summary>

The maximum distance to search for neighbors.

Default: `100`

📋 _Visible when Search Radius Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Additional offset added to the search radius value.

Default: `0`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory to connect to Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExProbeBitmasks.h)
