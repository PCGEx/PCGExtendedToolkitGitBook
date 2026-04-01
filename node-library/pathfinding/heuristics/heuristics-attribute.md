---
description: Read a vtx or edge attribute as an heuristic value.
icon: circle-dashed
---

# Heuristics : Attribute

### Overview

This heuristic reads an attribute from either vertices or edges and converts it into a score that influences pathfinding decisions. The attribute value can be used directly or remapped through a curve, allowing you to leverage existing data (like height, density, or custom properties) to guide path behavior.

### How It Works

1. **Source Selection**: Reads the attribute from either vertices (points) or edges (connections between points) based on the Source setting
2. **Value Processing**: Depending on the Mode, either normalizes the attribute value automatically, uses manual min/max bounds, or uses the raw value directly
3. **Score Caching**: Pre-computes all scores for the entire cluster for optimal pathfinding performance (FullyStatic category)
4. **Curve Application**: If using a curve mode, the normalized value (0-1) is fed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Input Attribute Values → Normalization → Curve Sampling → Heuristic Score

Example with AutoCurve mode:

Vertex Attribute: [10, 50, 100, 75, 25]
Auto Min/Max: 10, 100
Normalized: [0.0, 0.44, 1.0, 0.72, 0.17]
After Curve: [scores based on curve shape]
```

```
Example with Raw mode:

Vertex Attribute: [0.5, 0.8, 0.2]
Output Scores: [0.5, 0.8, 0.2] (no processing)
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExAttributeHeuristicInputMode</code></summary>

Specifies how to process the attribute value before using it as a score.

| Option           | Description                                                                                                                                             |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Auto Curve**   | Automatically normalizes the attribute value using its existing min/max range, then samples the curve. Safe and automatic.                              |
| **Manual Curve** | Normalizes using manually specified min/max values (InMin/InMax), then samples the curve. Use when you want explicit control over the input range.      |
| **Raw**          | Uses the raw attribute value directly as the score without any processing. Requires the attribute to already be in a valid score range (typically 0-1). |

Default: `Auto Curve`

</details>

<details>

<summary><strong>Source</strong> <code>EPCGExClusterElement</code></summary>

Determines whether to read the attribute from vertices (points) or edges (connections).

| Option   | Description                          |
| -------- | ------------------------------------ |
| **Vtx**  | Reads from vertex (point) attributes |
| **Edge** | Reads from edge attributes           |

Default: `Vtx`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read the modifier value from. Can be any numeric attribute on the selected source (vertices or edges).

⚡ PCG Overridable

</details>

<details>

<summary><strong>In Min</strong> <code>double</code></summary>

The minimum value to use when normalizing the attribute for curve sampling. Values at or below this will map to 0.0 when sampling the curve.

Default: `0`

📋 _Visible when Mode = Manual Curve_

⚡ PCG Overridable

</details>

<details>

<summary><strong>In Max</strong> <code>double</code></summary>

The maximum value to use when normalizing the attribute for curve sampling. Values at or above this will map to 1.0 when sampling the curve.

Default: `1`

📋 _Visible when Mode = Manual Curve_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Custom Fallback</strong> <code>bool</code></summary>

When enabled, uses a custom fallback value when normalization fails (e.g., all attribute values are identical, causing min to equal max).

Default: `false`

</details>

<details>

<summary><strong>Fallback Value</strong> <code>double</code></summary>

The score to use when normalization cannot be performed (e.g., all points have identical attribute values). If Use Custom Fallback is disabled, the system clamps the min/max between 0 and 1 automatically.

Default: `1`

Range: `0.0` to `1.0`

📋 _Visible when Use Custom Fallback is enabled_

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicAttribute.h)
