---
description: Heuristics based on steepness.
icon: circle-dashed
---

# Heuristics : Steepness

### Overview

This heuristic scores edges based on their steepness relative to a defined "up" direction (typically vertical). It measures how much an edge climbs or descends by calculating the alignment between the edge direction and the up vector. This enables terrain-aware pathfinding that can favor flat paths, avoid steep climbs, or seek dramatic elevation changes.

### How It Works

1. **Edge Direction Caching**: Pre-computes and caches normalized direction vectors for all edges in the cluster
2. **Steepness Calculation**: For each edge, calculates the dot product between the edge direction and the up vector, which measures how aligned the edge is with the vertical axis
3. **Absolute vs Signed**:
   * **Absolute Steepness**: Uses the magnitude only (0 = horizontal, 1 = vertical), so climbing up and going down are treated the same
   * **Signed Steepness**: Preserves direction (-1 = down, 0 = horizontal, +1 = up)
4. **Optional Accumulation**: If enabled, adds steepness from previous edges in the path to emphasize cumulative elevation change
5. **Curve Application**: The steepness value is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Up Vector: (0, 0, 1) - pointing upward

Edge A→B: Horizontal (→)       Dot = 0.0  (flat)
Edge C→D: 45° upward (↗)       Dot = 0.7  (moderate climb)
Edge E→F: Vertical upward (↑)  Dot = 1.0  (steep climb)
Edge G→H: 45° downward (↘)     Dot = -0.7 (moderate descent)
```

```
Absolute Steepness = true:

  A→B: 0.0 (flat)
  C→D: 0.7 (steep)
  E→F: 1.0 (steepest)
  G→H: 0.7 (steep, same as C→D)
```

```
Absolute Steepness = false:

  A→B: 0.5 (remapped from 0.0)
  C→D: 0.85 (remapped from 0.7, going up)
  E→F: 1.0 (remapped from 1.0, going up)
  G→H: 0.15 (remapped from -0.7, going down)
```

With **Accumulation**:

```
Path: A→B→C (each edge has steepness 0.3)
Without accumulation: Edge C→D scores 0.5
With accumulation (2 samples): Edge C→D scores 0.5 + 0.3 + 0.3 = 1.1
(Exacerbates cumulative elevation changes)
```

This heuristic is **Goal-Dependent** by default, or **Travel-Dependent** when accumulation is enabled.

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Accumulate Score</strong> <code>bool</code></summary>

When enabled, adds steepness from previous edges in the path to the current edge's score. This exacerbates cumulative elevation changes, making the heuristic more sensitive to sustained climbs or descents.

Useful for terrain with gradual slopes where individual edge steepness may be low but cumulative change is significant.

**Note**: Enabling this changes the heuristic category from Goal-Dependent to Travel-Dependent, which may impact performance.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Accumulation Samples</strong> <code>int32</code></summary>

Number of previous edges to include when accumulating steepness. Higher values create stronger emphasis on sustained elevation changes.

* **1**: Add only the immediate previous edge
* **2-3**: Consider recent climbing/descending trend
* **4+**: Strong sensitivity to cumulative elevation patterns

Default: `1`

Minimum: `1`

📋 _Visible when Accumulate Score is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Vector</strong> <code>FVector</code></summary>

The direction considered "up" for steepness calculations. Typically the world's vertical axis, but can be customized for non-standard gravity or angled terrain references.

The dot product measures how aligned each edge is with this vector. The vector is automatically mirrored internally.

Default: `FVector::UpVector` (0, 0, 1)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute Steepness</strong> <code>bool</code></summary>

Determines whether steepness direction matters.

* **True** (default): Uses absolute value, so steepness measures how vertical an edge is regardless of whether it goes up or down. A 45° climb and a 45° descent both score as 0.7 steepness.
* **False**: Preserves direction, where -1 = straight down, 0 = horizontal, +1 = straight up. The full range (-1 to +1) is remapped to (0 to 1) for curve sampling.

Use absolute for "avoid steep terrain regardless of direction." Use signed to "favor uphill" or "favor downhill" paths.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Combine with Distance or Azimuth to create paths that balance progress toward a goal with terrain steepness constraints.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicSteepness.h)
