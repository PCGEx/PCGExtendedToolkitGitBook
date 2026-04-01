---
description: >-
  Heuristics based on direction inertia from last visited node. NOTE: Can be
  quite expensive.
icon: circle-dashed
---

# Heuristics : Inertia

### Overview

This heuristic encourages paths to maintain their current direction of travel by favoring edges that align with the path's recent movement. It looks back at previously traversed edges to determine the "flow" direction, then scores potential next edges based on how well they continue that momentum. This creates smoother, more natural-looking paths that avoid sharp zigzags and prefer gentle curves.

### How It Works

1. **Travel History Retrieval**: Accesses the TravelStack to examine previously visited nodes and edges in the current path
2. **Direction Averaging**: Samples the last N edges (configured by Samples setting) and averages their direction vectors to determine the path's momentum
3. **Alignment Calculation**: Compares each candidate edge's direction against the averaged inertia direction using the dot product, which measures directional alignment
4. **Fallback Handling**: If there aren't enough previous edges to sample (e.g., at the start of a path), uses the configured fallback score
5. **Curve Application**: The alignment value is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Path so far: A → B → C (moving East)
Averaged direction: →

From C, evaluating options:

Edge C→D: → (East, aligned)     Dot = 1.0  High score
Edge C→E: ↗ (NE, partial)       Dot = 0.7  Medium score
Edge C→F: ↑ (North, perpendicular) Dot = 0.0  Low score
Edge C→G: ← (West, opposite)    Dot = -1.0 Very low score
```

```
Sample Count Impact:

Samples = 1: Only looks at B→C edge
Samples = 3: Averages A→B, B→C, and any prior edges
Higher samples = smoother, more gradual turns
```

This heuristic is **Travel-Dependent**, meaning it depends on the path taken to reach each node and cannot be pre-computed.

**Performance Note**: This heuristic requires examining the travel history for every edge evaluation, which can be computationally expensive for complex graphs or long paths.

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Samples</strong> <code>int32</code></summary>

Number of previous edges to average when computing the inertia direction. Higher values create smoother, more gradual directional changes.

* **1**: Only considers the immediate previous edge (sharp, responsive)
* **2-3**: Balances recent direction with some smoothing
* **4+**: Creates very smooth, flowing paths that resist direction changes

Default: `1`

Minimum: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore If Not Enough Samples</strong> <code>bool</code></summary>

When enabled, uses the fallback score if the path doesn't have enough previous edges to meet the Samples requirement (e.g., at path start or when Samples = 5 but only 3 edges have been traversed).

* **True** (default): Use Fallback Inertia Score when insufficient samples
* **False**: Compute inertia from whatever samples are available, even if less than requested

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Global Inertia Score</strong> <code>double</code></summary>

Fallback value used for the global score calculation. Primarily used by pathfinding algorithms (like A\*) for initial node sorting before any edges have been traversed.

Since there's no travel history at the global level, this provides a default inertia value.

Default: `0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fallback Inertia Score</strong> <code>double</code></summary>

Fallback heuristic score used when no inertia value can be computed. This typically happens at the start of a path (no previous edges) or when there aren't enough samples and Ignore If Not Enough Samples is enabled.

A value of 0.0 means no preference (neutral), while higher values would bias initial direction choices.

Default: `0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Combine with Distance or Azimuth heuristics to create paths that both progress toward a goal and maintain smooth, natural curvature.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicInertia.h)
