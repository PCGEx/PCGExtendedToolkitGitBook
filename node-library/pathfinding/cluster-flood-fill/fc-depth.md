---
description: Control fill based on diffusion depth.
icon: circle-dashed
---

# FC : Depth

### Overview

This fill control sub-node limits how far diffusion can spread from a seed by counting the number of edge traversals (hops). When the depth reaches the maximum, expansion stops. This creates a natural boundary based on graph topology rather than spatial distance.

### How It Works

1. **Track Depth**: Each candidate maintains a depth value (number of edges traversed from the seed).
2. **Check Limit**: Before accepting a candidate, checks if its depth would exceed the maximum.
3. **Stop at Boundary**: When depth limit is reached, no further expansion occurs in that direction.

**Usage Notes**

* **Topology-Based**: Depth counts edge traversals, not spatial distance. Two vertices 1 hop apart may be spatially far if connected by a long edge.
* **Uniform Spread**: Creates roughly circular regions in regular grids, but follows graph connectivity in irregular clusters.
* **Validates All Steps**: This control validates captures, probes, and candidates at all stages.

### Behavior

```
Depth Limit (MaxDepth = 3):

           Depth 0    Depth 1     Depth 2     Depth 3    Depth 4
             ↓           ↓           ↓          ↓          ↓
Seed ────→ [Vtx] ────→ [Vtx] ────→ [Vtx] ────→ [Vtx] ────→ [STOP]
                                                ↑
                                          Max depth reached

Graph Visualization:
                    ○───○───○
                    │   │   │
        Seed ───→   ●───○───○    (● = captured at depth ≤ 3)
                    │   │   │
                    ○───○───○
```

### Settings

<details>

<summary><strong>Max Depth Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the maximum depth comes from a constant or attribute.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use a fixed depth limit            |
| **Attribute** | Read depth limit from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Max Depth (Attr)</strong> <code>FName</code></summary>

Attribute name to read the depth limit from.

Default: `MaxDepth`

📋 _Visible when Max Depth Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Depth</strong> <code>int32</code></summary>

Maximum number of edge traversals from the seed before stopping.

Default: `10`

Minimum: 1

📋 _Visible when Max Depth Input = Constant_

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source, Steps

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlDepth.h)
