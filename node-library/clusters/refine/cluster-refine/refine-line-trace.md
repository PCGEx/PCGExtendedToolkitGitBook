---
description: Removes edges based on line trace collision with world geometry.
icon: function
---

# Refine : Line Trace

### Overview

This edge refinement operation performs line traces (raycasts) along each edge to detect collisions with world geometry. Edges that hit collision surfaces are removed by default, allowing you to prune connections that would pass through obstacles. This is useful for creating clusters that respect the physical environment.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Edge Iteration**: Processes each edge in the cluster.
2. **Line Trace**: Performs a raycast from one endpoint to the other using the configured collision settings.
3. **Hit Detection**: Checks if the trace hits any collision geometry.
4. **Edge Removal**: Edges with collisions are removed (or kept if inverted).

**Usage Notes**

* **Two-Way Check**: Enable this to trace in both directions, avoiding false negatives from backfacing surfaces.
* **Scatter Mode**: When environment geometry is complex, scatter multiple traces around the endpoint for more reliable detection.
* **World Geometry**: This only detects collision with world actors — it does not detect collision between cluster edges themselves.

### Behavior

```
Before:                         After (edges hitting walls removed):

    A---B                       A---B
    |   |                       |
    | W |  (W = wall)           | W |
    |   |                       |
    C---D                       C   D

Edge B-D removed (hits wall W)
Edge A-C kept (no collision)
```

### Settings

<details>

<summary><strong>Collision Settings</strong> <code>FPCGExCollisionDetails</code></summary>

Configuration for the line trace collision detection, including collision channel, trace type, and actor filtering options.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Two Way Check</strong> <code>bool</code></summary>

If the first line trace fails to detect collision, tries tracing in the opposite direction. This helps detect backfacing surfaces that might be missed with a single trace, at the cost of additional processing.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scatter</strong> <code>bool</code></summary>

Enable scatter mode to perform multiple traces around the endpoint for improved hit detection reliability in complex environments.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Samples</strong> <code>double</code></summary>

Number of scatter trace samples to perform when scatter mode is enabled.

Default: `10`

📋 _Visible when Scatter is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Radius around the endpoint within which scatter samples are randomly distributed.

Default: `10`

📋 _Visible when Scatter is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the refinement result. When enabled, edges that hit collision are kept while edges with no collision are removed.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineLineTrace.h)
