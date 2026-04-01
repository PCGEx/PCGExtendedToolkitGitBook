---
description: >-
  Physics-based relaxation using Verlet integration with gravity, friction, and
  edge length constraints.
icon: function
---

# Verlet (Gravity)

### Overview

This relaxation operation simulates physical behavior using Verlet integration. Vertices are affected by gravity and momentum, while edges act as constraints trying to maintain their rest lengths. Friction can pin vertices in place or slow their movement. The result is natural-looking "sagging" behavior similar to rope or cloth physics.

> This is a [relax-operation.md](relax-operation.md "mention")

### How It Works

1. **Velocity Integration (Step 1)**: Each vertex's velocity is computed from the difference between current and previous positions, then gravity is applied.
2. **Constraint Solving (Step 2)**: Edge constraints push/pull connected vertices to maintain target edge lengths.
3. **Position Update (Step 3)**: Accumulated corrections are applied to vertex positions.
4. **Iteration**: The process repeats, with damping gradually reducing motion until equilibrium.

**Usage Notes**

* **Gravity**: Acts as a constant force pulling vertices in the specified direction. Use for natural sagging effects.
* **Friction**: Values of 1.0 completely pin a vertex in place — useful for anchor points.
* **Edge Stiffness**: Higher values make edges more rigid; lower values allow more stretch.
* **Damping**: Controls how quickly motion settles. Lower values converge faster but may look less natural.

### Behavior

```
Before (flat):                  After (gravity applied):

    A---B---C---D               A           D
                                 \         /
                                  B-------C
                                  (sagging under gravity)

With Friction=1 on A and D, they stay pinned while B and C sag.
```

### Settings

#### Gravity

<details>

<summary><strong>Gravity Input</strong> <code>EPCGExInputValueType</code></summary>

How gravity is determined for each vertex.

| Option        | Description                          |
| ------------- | ------------------------------------ |
| **Constant**  | Same gravity vector for all vertices |
| **Attribute** | Read gravity from a point attribute  |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Gravity</strong> <code>FVector</code></summary>

The gravity vector applied to all vertices. Negative Z creates downward pull.

Default: `(0, 0, -100)`

📋 _Visible when Gravity Input = Constant_

⚡ PCG Overridable

</details>

#### Friction

<details>

<summary><strong>Friction Input</strong> <code>EPCGExInputValueType</code></summary>

How friction is determined for each vertex.

| Option        | Description                          |
| ------------- | ------------------------------------ |
| **Constant**  | Same friction for all vertices       |
| **Attribute** | Read friction from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Friction</strong> <code>double</code></summary>

Friction coefficient in the range \[0, 1]. A value of 0 means no friction (free movement), while 1 completely pins the vertex in place.

Default: `0`

📋 _Visible when Friction Input = Constant_

⚡ PCG Overridable

</details>

#### Edge Constraints

<details>

<summary><strong>Edge Scaling Input</strong> <code>EPCGExInputValueType</code></summary>

How edge rest length scaling is determined.

| Option        | Description                         |
| ------------- | ----------------------------------- |
| **Constant**  | Same scaling for all edges          |
| **Attribute** | Read scaling from an edge attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Scaling</strong> <code>double</code></summary>

Multiplier applied to edge rest lengths. Values greater than 1 allow edges to be longer than their initial length.

Default: `1`

📋 _Visible when Edge Scaling Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Stiffness Input</strong> <code>EPCGExInputValueType</code></summary>

How edge stiffness is determined.

| Option        | Description                           |
| ------------- | ------------------------------------- |
| **Constant**  | Same stiffness for all edges          |
| **Attribute** | Read stiffness from an edge attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Stiffness</strong> <code>double</code></summary>

Edge constraint stiffness in the range \[0, 1]. Higher values make edges more rigid; lower values allow more stretching.

Default: `0.5`

📋 _Visible when Edge Stiffness Input = Constant_

⚡ PCG Overridable

</details>

#### Simulation

<details>

<summary><strong>Time Step</strong> <code>double</code></summary>

The simulation time step for each iteration. Affects how quickly the system evolves and the strength of gravity's effect.

Default: `0.1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Damping Scale</strong> <code>double</code></summary>

Velocity damping multiplier applied each iteration. Lower values provide more damping for smoother convergence. Higher values retain momentum for more natural motion and sag.

Default: `0.99`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExVerletRelax.h)
