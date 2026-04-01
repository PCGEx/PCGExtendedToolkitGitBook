---
description: Deactivates output pin if all inputs are empty or missing.
icon: circle
---

# Cull On Empty

### Overview

Cull On Empty inspects all incoming data and deactivates its output pin if nothing contains actual content. Non-empty inputs are forwarded as-is. This prevents downstream nodes from executing when there's nothing to work with — useful for conditional branches where empty data should halt the pipeline rather than produce empty results.

### How It Works

1. **Inspect Inputs**: Each input is checked for content based on its type:
   * **Point data**: must have at least one point
   * **Spline/polyline data**: must have at least one segment
   * **Attribute sets**: must have at least one metadata entry
   * **Other data types**: considered non-empty if they exist
2. **Forward Non-Empty**: Any input that passes the content check is forwarded to the output.
3. **Deactivate If Empty**: If _no_ inputs contain valid data, the output pin is deactivated. This signals the PCG graph that downstream execution should be skipped.

**Usage Notes**

* **Pin deactivation**: A deactivated output pin tells the PCG graph to skip all downstream nodes connected to it. This is different from forwarding empty data, which would still trigger downstream execution.
* **Mixed inputs**: If some inputs are empty and others are not, only the non-empty ones are forwarded. The pin stays active.
* **Dynamic pins**: This node uses dynamic pins — the input and output accept any PCG data type.

### Behavior

```
Inputs (all empty)         Output
┌─────────────────┐       ┌──────────────────┐
│ Points: 0       │  →    │ Pin: DEACTIVATED │
│ Spline: 0 segs  │       └──────────────────┘
└─────────────────┘

Inputs (some non-empty)    Output
┌─────────────────┐       ┌──────────────────┐
│ Points: 0       │  →    │ Points: 50 (fwd) │
│ Points: 50      │       │ Pin: ACTIVE      │
└─────────────────┘       └──────────────────┘
```

### Inputs

| Pin    | Type | Description                                          |
| ------ | ---- | ---------------------------------------------------- |
| **In** | Any  | Any PCG data — points, splines, attribute sets, etc. |

### Settings

This node has no configurable settings. It passes through non-empty data and deactivates the output pin when everything is empty.

### Outputs

| Pin     | Type | Description                                                              |
| ------- | ---- | ------------------------------------------------------------------------ |
| **Out** | Any  | Non-empty inputs forwarded. Pin is deactivated if all inputs were empty. |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExCullOnEmpty.h)
