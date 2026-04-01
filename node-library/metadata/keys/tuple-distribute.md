---
description: Distribute weighted tuple row values across input points.
icon: circle
---

# Tuple : Distribute

### Overview

Assigns attribute values from a weighted table to input points. You define a set of columns (the composition) and rows (each with a weight and per-column values), and the node distributes rows across points using index-based, random, or weighted random selection. Each point receives the attribute values from its assigned row.

### How It Works

1. **Define columns**: The **Composition** defines the output attributes — their names and types. These are the columns of your tuple table.
2. **Define rows**: Each entry in **Values** is a row with a **Weight** and per-column value overrides. Individual columns can be toggled on/off per row.
3. **Distribute**: For each input point, a row is picked based on the selected distribution mode. The row's column values are written to the point as attributes.
4. **Optional outputs**: The picked row index and/or weight can be written as additional attributes for downstream use.

**Usage Notes**

* **Auto-sync**: Rows automatically sync when the composition changes in the editor. Adding or removing columns updates all existing rows.
* **Per-column toggles**: Each column in a row can be individually enabled or disabled. Disabled columns are skipped during write, leaving the point's existing value (or default) untouched.
* **Zero weights**: If all rows have weight 0 in weighted random mode, the node falls back to uniform random distribution.
* **Data stealing**: This node supports data stealing for zero-copy operation when you don't need to preserve the original input data.

### Behavior

```
Composition (columns):     Color (FVector)    Size (double)    Tag (FName)
                           ────────────────   ──────────────   ───────────
Row 0  (Weight: 3)         (1, 0, 0)          2.0              "TypeA"
Row 1  (Weight: 1)         (0, 1, 0)          1.0              "TypeB"
Row 2  (Weight: 1)         (0, 0, 1)          0.5              "TypeC"

Distribution: WeightedRandom
→ Row 0 picked ~60% of the time, Row 1 and 2 ~20% each

Each point receives:  Color, Size, Tag attributes from its picked row
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Composition</strong> <code>FPCGExPropertySchemaCollection</code></summary>

Defines the columns of the tuple table — the output attribute names and types. Each schema entry becomes one attribute written to points.

</details>

<details>

<summary><strong>Values</strong> <code>TArray&#x3C;FPCGExWeightedPropertyOverrides></code></summary>

The rows of the tuple table. Each row contains:

* **Weight** (`int32`, default: `1`) — relative probability for weighted random distribution
* **Overrides** — per-column values, auto-synced to match the composition. Each column can be individually toggled on/off.

</details>

<details>

<summary><strong>Distribution</strong> <code>EPCGExDistribution</code></summary>

How rows are assigned to points.

| Option              | Description                                                 |
| ------------------- | ----------------------------------------------------------- |
| **Index**           | Deterministic — each point picks the row matching its index |
| **Random**          | Uniform random — equal probability regardless of weight     |
| **Weighted Random** | Probability proportional to each row's weight               |

Default: `WeightedRandom`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle point indices that exceed the number of rows.

| Option     | Description                             |
| ---------- | --------------------------------------- |
| **Ignore** | Points beyond the row count are skipped |
| **Tile**   | Wraps around (modulo)                   |
| **Clamp**  | Clamps to the last row                  |
| **Yoyo**   | Ping-pongs back and forth               |

Default: `Tile`

📋 _Visible when Distribution = Index_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Seed Components</strong> <code>uint8</code> (bitmask)</summary>

Which components contribute to seed generation for random distribution.

Default: `Local | Settings`

📋 _Visible when Distribution is not Index_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Local Seed</strong> <code>int32</code></summary>

Additional seed offset for random distribution.

Default: `0`

📋 _Visible when Distribution is not Index_

⚡ PCG Overridable

</details>

***

**Additional Outputs**

<details>

<summary><strong>Output Row Index</strong> <code>bool</code></summary>

When enabled, writes the picked row index as an attribute on each point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Row Index Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to write the picked row index to.

Default: `TupleRowIndex`

📋 _Visible when Output Row Index is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Weight</strong> <code>bool</code></summary>

When enabled, writes the picked row's weight as an attribute on each point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to write the picked row's weight to.

Default: `TupleWeight`

📋 _Visible when Output Weight is enabled_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common point processor settings from its base class.

> See Points Processor Settings for shared settings.

### Outputs

| Pin     | Type   | Description                                              |
| ------- | ------ | -------------------------------------------------------- |
| **Out** | Points | Input points with tuple row values written as attributes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExDistributeTuple.h)
