---
description: Base class for all heuristic computational nodes used in pathfinding.
icon: arrow-down-from-arc
---

# Heuristics Definition

### Overview

This is the abstract base class that defines the common structure and settings for all heuristic nodes in PCGExtended. Heuristics are scoring functions used by pathfinding algorithms to evaluate and prioritize different path options. Each heuristic type inherits the settings documented here and may add its own specific properties.

### How It Works

1. **Configuration**: Heuristics are created as factory nodes that output heuristic definitions.
2. **Weight Application**: The WeightFactor scales the heuristic's contribution relative to other heuristics.
3. **Score Remapping**: Raw heuristic scores are remapped through a curve to control their distribution.
4. **Local Modulation**: Optional per-vertex or per-edge attributes can multiply the heuristic weight locally.
5. **Pathfinding Integration**: Multiple heuristics can be combined in pathfinding nodes to guide path selection.

### Behavior

**Weight Factor Impact:**

```
Heuristic A (Weight=1.0): Score range [0-100]
Heuristic B (Weight=0.5): Score range [0-50]  (half influence)
Heuristic C (Weight=2.0): Score range [0-200] (double influence)
```

**Score Curve Remapping:**

```
Raw Score:    0.0  0.25  0.5  0.75  1.0
Linear Curve: 0.0  0.25  0.5  0.75  1.0 (unchanged)
Ease In:      0.0  0.06  0.25 0.56  1.0 (favor high scores)
Ease Out:     0.0  0.44  0.75 0.94  1.0 (favor low scores)
```

### Base Settings

All heuristic nodes inherit these settings unless otherwise noted.

#### Core Settings

<details>

<summary><strong>Weight Factor</strong> <code>double</code></summary>

The weight factor for this heuristic. Multiplies the final score to control this heuristic's influence relative to other heuristics.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the final heuristics score. When enabled, high scores become low and vice versa, effectively reversing the heuristic's preference.

Default: `false`

⚡ PCG Overridable

📋 _Hidden when Raw Settings is enabled_

</details>

#### Score Curve Settings

<details>

<summary><strong>Use Local Curve</strong> <code>bool</code></summary>

Whether to use in-editor curve or an external asset. When enabled, uses the embedded curve editor. When disabled, uses an external curve asset.

Default: `false`

📋 _Hidden when Raw Settings is enabled_

</details>

<details>

<summary><strong>Score Curve (Local)</strong> <code>FRuntimeFloatCurve</code></summary>

Curve the value will be remapped over. Embedded curve editor for remapping raw heuristic scores to final scores.

Default: Linear curve from (0,0) to (1,1)

📋 _Visible when Use Local Curve = true and Raw Settings = false_

</details>

<details>

<summary><strong>Score Curve (Asset)</strong> <code>TSoftObjectPtr</code></summary>

Curve the value will be remapped over. External curve asset for remapping raw heuristic scores to final scores.

Default: `WeightDistributionLinear`

⚡ PCG Overridable

📋 _Visible when Use Local Curve = false and Raw Settings = false_

</details>

<details>

<summary><strong>Score Curve Lookup</strong> <code>FPCGExCurveLookupDetails</code></summary>

Additional details for curve lookup behavior and sampling.

Default: Default lookup settings

</details>

#### Local Weight Multiplier Settings

<details>

<summary><strong>Use Local Weight Multiplier</strong> <code>bool</code></summary>

Use a local attribute to multiply the heuristic weight on a per-element basis. Allows spatial variation of heuristic influence.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Local Weight Multiplier Source</strong> <code>EPCGExClusterElement</code></summary>

Local multiplier attribute source. Specifies whether to read the multiplier from vertex or edge attributes.

| Option   | Description                        |
| -------- | ---------------------------------- |
| **Vtx**  | Read from vertex (node) attributes |
| **Edge** | Read from edge attributes          |

Default: `Vtx`

⚡ PCG Overridable

📋 _Visible when Use Local Weight Multiplier = true_

</details>

<details>

<summary><strong>Weight Multiplier Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read multiplier value from. The attribute value will multiply the heuristic's weight at each element.

⚡ PCG Overridable

📋 _Visible when Use Local Weight Multiplier = true_

</details>

#### Roaming Context Settings

<details>

<summary><strong>UVW Seed</strong> <code>FVector</code></summary>

Bound-relative seed position used when this heuristic is used in a "roaming" context. Specifies seed location as normalized coordinates within graph bounds (0-1 range per axis).

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>UVW Goal</strong> <code>FVector</code></summary>

Bound-relative goal position used when this heuristic is used in a "roaming" context. Specifies goal location as normalized coordinates within graph bounds (0-1 range per axis).

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

### Outputs

| Pin            | Type            | Description                                                           |
| -------------- | --------------- | --------------------------------------------------------------------- |
| **Heuristics** | PCGEx Heuristic | Outputs the configured heuristic factory for use in pathfinding nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Core/PCGExHeuristicsFactoryProvider.h)
