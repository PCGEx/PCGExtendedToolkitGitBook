---
description: Stop fill after a certain number of vtx have been captured.
icon: circle-dashed
---

# FC : Count

### Overview

This fill control sub-node limits how many vertices each seed can capture during flood fill. Once a seed has claimed the maximum number of vertices, it stops expanding further. This provides a simple way to constrain the size of each diffusion region.

### How It Works

1. **Track Captures**: Counts the number of vertices each diffusion has successfully captured.
2. **Check Limit**: Before capturing a new vertex, checks if the count would exceed the maximum.
3. **Stop When Full**: When the limit is reached, the diffusion stops accepting new vertices.

**Usage Notes**

* **Per-Seed Limit**: Each seed has its own independent count. If Max Count is 10, each seed can capture up to 10 vertices.
* **Capture Only**: This control only validates captures (when a vertex is claimed), not probes or candidates.
* **Attribute-Driven**: Use attribute input to give different seeds different capture limits.

### Behavior

```
Count Limit (MaxCount = 5):

Seed → [1] → [2] → [3] → [4] → [5] → STOP
        ↓     ↓     ↓     ↓     ↓
Count:  1     2     3     4     5 (limit reached)

Multiple Seeds with same limit:
  Seed A: captures 5 vertices, then stops
  Seed B: captures 5 vertices, then stops
  (Total: 10 vertices captured)
```

### Settings

<details>

<summary><strong>Max Count Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the maximum count comes from a constant or attribute.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use a fixed count limit            |
| **Attribute** | Read count limit from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Max Count (Attr)</strong> <code>FName</code></summary>

Attribute name to read the count limit from.

Default: `MaxCount`

📋 _Visible when Max Count Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Count</strong> <code>int32</code></summary>

Maximum number of vertices each seed can capture before stopping.

Default: `10`

Minimum: 1

📋 _Visible when Max Count Input = Constant_

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source, Steps

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlCount.h)
