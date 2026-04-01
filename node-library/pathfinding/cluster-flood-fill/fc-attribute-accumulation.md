---
description: Track accumulated attribute value along path, stop when threshold exceeded.
icon: circle-dashed
---

# FC : Attribute Accumulation

### Overview

This fill control sub-node tracks an attribute value as it accumulates along the diffusion path. It can sum, track maximum, track minimum, or compute a running average of values encountered. When the accumulated value exceeds a configurable threshold, further expansion along that path is stopped.

### How It Works

1. **Read Attribute**: For each candidate vertex or edge, reads the specified attribute value.
2. **Accumulate**: Updates the running total based on the selected mode (sum, max, min, average).
3. **Check Threshold**: Compares accumulated value against the maximum threshold.
4. **Stop or Continue**: If threshold is exceeded, marks the candidate as invalid to stop expansion in that direction.

**Usage Notes**

* **Scoring**: This control also scores candidates based on accumulated values, influencing priority when multiple paths compete.
* **Accumulated Value Storage**: The accumulated value is stored in the candidate state and can be read by other fill controls.
* **Edge vs Vertex**: The attribute can be read from either vertices or edges, depending on your data layout.

### Behavior

```
Accumulation Modes:

Sum Mode (MaxAccumulation = 50):
  Seed → [10] → [15] → [20] → [10] → STOP
              10     25     45     55 (exceeds 50)

Max Mode (MaxAccumulation = 30):
  Seed → [10] → [25] → [15] → [35] → STOP
              10     25     25     35 (exceeds 30)

Min Mode (MaxAccumulation = 5):
  Seed → [10] → [8] → [12] → [3] → STOP
              10     8      8      3 (below threshold, but inverted logic)

Average Mode (MaxAccumulation = 20):
  Seed → [10] → [30] → [10] → [30] → STOP
              10     20     16.7   20 (hits threshold)
```

### Settings

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read and accumulate along the diffusion path.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attribute Source</strong> <code>EPCGExClusterElement</code></summary>

Where to read the attribute from.

| Option   | Description                |
| -------- | -------------------------- |
| **Vtx**  | Read from cluster vertices |
| **Edge** | Read from cluster edges    |

Default: `Vtx`

</details>

<details>

<summary><strong>Mode</strong> <code>EPCGExAccumulationMode</code></summary>

How values are accumulated along the path.

| Option      | Description                           |
| ----------- | ------------------------------------- |
| **Sum**     | Add all values together               |
| **Maximum** | Track the highest value encountered   |
| **Minimum** | Track the lowest value encountered    |
| **Average** | Compute running average of all values |

Default: `Sum`

</details>

<details>

<summary><strong>Max Accumulation Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the threshold comes from a constant or attribute.

| Option        | Description                      |
| ------------- | -------------------------------- |
| **Constant**  | Use a fixed threshold value      |
| **Attribute** | Read threshold from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Max Accumulation (Attr)</strong> <code>FName</code></summary>

Attribute name to read the threshold from.

Default: `MaxAccumulation`

📋 _Visible when Max Accumulation Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Accumulation</strong> <code>double</code></summary>

The threshold value. When accumulation exceeds this, expansion stops.

Default: `100.0`

📋 _Visible when Max Accumulation Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write to Accumulated Value</strong> <code>bool</code></summary>

When enabled, stores the accumulated value in the candidate state for use by other fill controls.

Default: `true`

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source, Steps

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlAttributeAccumulation.h)
