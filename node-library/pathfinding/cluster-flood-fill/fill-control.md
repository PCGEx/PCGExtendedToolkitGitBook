---
description: Creates a single Fill Control node, to be used with flood fill nodes.
icon: arrow-down-from-arc
---

# Fill Control

### Overview

This abstract base class defines the foundation for fill control sub-nodes used with flood fill operations. Fill controls regulate how the flood fill algorithm expands across a cluster graph by providing conditions, limits, and behaviors that determine when filling should continue or stop at each step.

### How It Works

1. **Factory Creation**: The provider settings create a factory data object containing the control configuration.
2. **Operation Spawning**: During flood fill execution, the factory spawns fill control operations that evaluate conditions.
3. **Step Evaluation**: Controls are applied at specified steps (seed, candidate, or both) to determine fill behavior.
4. **Source Selection**: Controls can read attribute values from either seed points or the points being evaluated.

### Base Configuration

All fill control sub-nodes inherit these common settings:

<details>

<summary><strong>Source</strong> <code>EPCGExFloodFillSettingSource</code></summary>

Determines where attribute values are read from during evaluation.

| Option      | Description                                          |
| ----------- | ---------------------------------------------------- |
| **Seed**    | Read values from the originating seed point          |
| **Current** | Read values from the point currently being evaluated |

Default: `Seed`

Note: Not all controls support source selection.

</details>

<details>

<summary><strong>Steps</strong> <code>EPCGExFloodFillControlStepsFlags</code></summary>

Bitmask controlling at which diffusion steps the control is applied.

| Flag          | Description                                          |
| ------------- | ---------------------------------------------------- |
| **Seed**      | Apply when evaluating the initial seed point         |
| **Candidate** | Apply when evaluating candidate points for expansion |

Default: `Candidate`

Note: Not all controls support step selection.

</details>

### Available Fill Controls

Fill control sub-nodes are connected to flood fill nodes via the Fill Controls input pin. Multiple controls can be combined to create complex fill behaviors.

→ See the Fill Controls category for specific control types:

* Attribute Accumulation
* Attribute Threshold
* Count
* Depth
* Edge Filters
* Heuristics Budget
* Heuristics Scoring
* Heuristics Threshold
* Keep Direction
* Length
* Running Average
* Vertex Filters

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/Core/PCGExFillControlsFactoryProvider.h)
