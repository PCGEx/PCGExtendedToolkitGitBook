---
description: Configures the influence (strength) of relaxation operations.
icon: sliders-simple
---

# Influence Details

### Overview

This settings block controls how strongly relaxation affects point positions. Influence determines the blend between original and relaxed positions, with 0 meaning no movement and 1 meaning full relaxation. The influence can be constant or per-point via attribute, and can optionally scale progressively with iteration count.

### How It Works

1. **Set Influence**: Define base influence strength (0-1)
2. **Choose Source**: Use constant value or read from attribute
3. **Enable Progression**: Optionally scale influence over iterations
4. **Apply Per-Iteration**: Each relaxation step applies the configured influence

### Behavior

```
Influence Effect:

Original Position ●                    Relaxed Target ○

Influence = 0.0:  ●                    (no movement)
Influence = 0.5:  ─────●─────          (halfway)
Influence = 1.0:            ○          (full relaxation)

Progressive Influence (over 4 iterations):
  Iteration 1: 0.25 influence
  Iteration 2: 0.50 influence
  Iteration 3: 0.75 influence
  Iteration 4: 1.00 influence (full configured amount)
```

### Settings

<details>

<summary><strong>Influence Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether influence is a constant or read from an attribute.

| Option        | Description                                |
| ------------- | ------------------------------------------ |
| **Constant**  | Use a fixed influence value for all points |
| **Attribute** | Read per-point influence from an attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Influence</strong> <code>double</code></summary>

The strength of relaxation effect. 0 = no movement, 1 = full relaxation to target position. Values between create a blend.

Default: `1.0`

📋 _Visible when Influence Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Progressive Influence</strong> <code>bool</code></summary>

When enabled, influence scales progressively from 0 to the configured value over the course of iterations. This creates smoother relaxation that gradually strengthens rather than applying full force immediately.

Default: `true`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Details/PCGExInfluenceDetails.h)
