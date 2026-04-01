---
description: Filter that checks vertices during diffusion.
icon: circle-dashed
---

# FC : Vtx Filters

### Overview

This fill control sub-node restricts which vertices diffusion can visit by applying point filter sub-nodes to vertex data. Only vertices that pass all connected filters can be captured by the diffusion. This enables creating barriers or allowed regions based on vertex attributes.

### How It Works

1. **Receive Filters**: Accepts point filter sub-nodes via the Filters input pin.
2. **Evaluate Vertices**: For each candidate vertex during diffusion, runs all filters against the vertex data.
3. **Block or Allow**: If any filter fails, diffusion cannot capture that vertex.

**Usage Notes**

* **Vertex Attributes**: Filters evaluate vertex point data, so use vertex attributes for conditions.
* **No Source Setting**: This control does not support the Source setting since it always evaluates vertices.
* **Compound Filters**: Connect multiple filters to create complex vertex conditions (all must pass).
* **Validates All Steps**: This control validates captures, probes, and candidates at all stages.

### Behavior

```
Vertex Filtering:

Cluster with vertex attributes (allowed = true/false):

    [T]───[T]───[F]───[T]───[T]
     │     │     │     │     │
    [T]───[F]───[T]───[T]───[F]
     │     │     │     │     │
    [T]───[T]───[T]───[F]───[T]

    T = allowed=true, F = allowed=false

Diffusion from top-left with filter "allowed == true":

    [●]───[●]───[✗]   [○]───[○]
     │     │     │     │     │
    [●]───[✗]   [○]   [○]───[✗]
     │     │     │     │     │
    [●]───[●]───[●]───[✗]   [○]

    ● = captured, ✗ = blocked by filter, ○ = unreachable
```

### Inputs

| Pin         | Type   | Description                                         |
| ----------- | ------ | --------------------------------------------------- |
| **Filters** | Params | Point filter sub-nodes to evaluate against vertices |

### Settings

This control has no node-specific settings beyond the base configuration.

**Inherited Settings**

→ See Fill Controls Base for: Steps

Note: The Source setting is not available for this control as it always evaluates vertex data.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlVtxFilters.h)
