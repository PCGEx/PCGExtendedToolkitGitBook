---
description: Heuristics based on attribute gradient between nodes.
icon: circle-dashed
---

# Heuristics : Gradient

### Overview

This heuristic scores edges based on how an attribute value changes from one node to another. It reads an attribute from vertices and evaluates whether values are increasing, decreasing, or staying constant along each edge. This enables behaviors like following uphill/downhill slopes, maintaining consistent attribute values, or seeking areas of maximum variation.

### How It Works

1. **Value Caching**: Pre-reads and caches the attribute value for every vertex in the cluster
2. **Gradient Calculation**: For each edge being evaluated, calculates the change in attribute value: `To Value - From Value`
3. **Optional Distance Normalization**: If enabled, divides the gradient by edge length to get gradient per unit distance (useful for comparing edges of different lengths)
4. **Mode-Based Scoring**:
   * **Follow Increasing**: Better scores for positive gradients (uphill)
   * **Follow Decreasing**: Better scores for negative gradients (downhill)
   * **Avoid Change**: Better scores for small gradients (stay on plateau)
   * **Seek Change**: Better scores for large gradients (maximize variation)
5. **Normalization and Curve**: The gradient value is normalized using Min/Max Gradient settings (for Avoid/Seek modes), then passed through the inherited Score Curve

### Behavior

```
Vertex Attribute (e.g., height): A=10, B=15, C=12

Edge A→B: Gradient = +5 (increasing)
Edge B→C: Gradient = -3 (decreasing)
Edge A→C: Gradient = +2 (slight increase)

Mode: Follow Increasing
  A→B (+5): High score  ← Preferred
  A→C (+2): Medium score
  B→C (-3): Low score

Mode: Follow Decreasing
  B→C (-3): High score  ← Preferred
  A→C (+2): Low score
  A→B (+5): Low score

Mode: Avoid Change
  A→C (+2): High score  ← Preferred (smallest change)
  B→C (-3): Medium score
  A→B (+5): Low score (largest change)
```

With **Normalize By Distance**:

```
Edge A→B: Gradient +5, Length 10 → Normalized: 0.5/unit
Edge C→D: Gradient +5, Length 5  → Normalized: 1.0/unit
(Both have same value change, but C→D is steeper)
```

This heuristic is **Fully Static**, meaning scores are pre-computed once per cluster.

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExGradientMode</code></summary>

Determines how to interpret the gradient between nodes.

| Option                | Description                                                                                                                      |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Follow Increasing** | Prefer edges where the attribute value increases (To > From). Paths will "climb" toward higher values.                           |
| **Follow Decreasing** | Prefer edges where the attribute value decreases (To < From). Paths will "descend" toward lower values.                          |
| **Avoid Change**      | Prefer edges where the attribute value stays similar. Penalizes large changes, keeping paths on "plateaus" of consistent values. |
| **Seek Change**       | Prefer edges where the attribute value changes significantly. Paths will favor areas of maximum variation.                       |

Default: `Follow Increasing`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The vertex attribute to read values from. Must be a numeric attribute present on the vertices (points) in the cluster.

The gradient is calculated as the change in this attribute's value from one vertex to the next.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normalize By Distance</strong> <code>bool</code></summary>

When enabled, divides the gradient by edge length to get gradient per unit distance. This makes scores comparable across edges of different lengths.

* **False** (default): Gradient = `To Value - From Value`
* **True**: Gradient = `(To Value - From Value) / Edge Length`

Enable this when edge lengths vary significantly and you want to measure steepness rather than absolute change.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Gradient</strong> <code>double</code></summary>

Expected minimum gradient value for normalization. Used to remap gradient values into the 0-1 range before curve sampling.

Only relevant for Avoid Change and Seek Change modes, where the absolute magnitude of change matters rather than its direction.

Default: `0`

📋 _Visible when Mode = Avoid Change or Seek Change_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Gradient</strong> <code>double</code></summary>

Expected maximum gradient value for normalization. Used to remap gradient values into the 0-1 range before curve sampling.

Only relevant for Avoid Change and Seek Change modes, where the absolute magnitude of change matters rather than its direction.

Default: `1`

📋 _Visible when Mode = Avoid Change or Seek Change_

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Combine with other heuristics like Distance to create paths that both move toward a goal and follow terrain slopes.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicGradient.h)
