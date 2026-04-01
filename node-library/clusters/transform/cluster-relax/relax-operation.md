---
description: >-
  Abstract base class for fitting-based cluster relaxation operations that use
  physics simulation principles.
icon: arrow-down-from-arc
---

# Relax Operation

### Overview

Fitting relaxation operations use a physics-inspired approach combining spring forces along edges with repulsion forces between vertices. This base class provides the common infrastructure for edge length preservation, repulsion, and iterative position updates. Derived classes implement specific collision detection strategies (box fitting, radius fitting, etc.).

### How It Works

1. **Spring Forces (Step 1)**: For each edge, calculates a spring force based on the difference between current and desired edge length.
2. **Repulsion Forces (Step 2)**: Derived classes implement specific overlap/proximity detection and apply repulsion forces.
3. **Position Update (Step 3)**: Accumulated forces are applied to vertex positions, scaled by the time step.

**Usage Notes**

* **Edge Fitting**: Controls whether and how edge lengths are preserved during relaxation.
* **Spring vs Repulsion Balance**: Adjust SpringConstant and RepulsionConstant to balance edge preservation against overlap resolution.
* **Time Step**: Smaller values give more stable but slower convergence; larger values converge faster but may overshoot.

### Settings

<details>

<summary><strong>Repulsion Constant</strong> <code>double</code></summary>

Controls the strength of repulsion forces when vertices overlap or are too close. Higher values push overlapping vertices apart more aggressively.

Default: `100`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Fitting</strong> <code>EPCGExRelaxEdgeFitting</code></summary>

Determines how edge lengths are handled during relaxation.

| Option        | Description                                                           |
| ------------- | --------------------------------------------------------------------- |
| **Ignore**    | Don't apply spring forces to edges — only repulsion affects positions |
| **Fixed**     | All edges aim for the same constant length                            |
| **Existing**  | Preserve each edge's original length                                  |
| **Attribute** | Read target length from an edge attribute                             |

Default: `Existing`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Desired Edge Length</strong> <code>double</code></summary>

The constant edge length all edges will try to maintain.

Default: `100`

📋 _Visible when Edge Fitting = Fixed_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Desired Edge Length (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute on edges specifying the target length for each edge.

📋 _Visible when Edge Fitting = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale</strong> <code>double</code></summary>

Scale factor applied to edge lengths. Values greater than 1 will try to expand edges beyond their target length.

Default: `2`

📋 _Visible when Edge Fitting = Existing or Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spring Constant</strong> <code>double</code></summary>

Stiffness of the edge springs. Lower values allow more edge length variation in favor of better overlap resolution. Higher values preserve edge topology more strictly.

Default: `0.1`

📋 _Visible when Edge Fitting is not Ignore_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Time Step</strong> <code>double</code></summary>

The simulation time step for each iteration. Smaller values give more stable but slower convergence; larger values converge faster but may overshoot or oscillate.

Default: `0.01`

⚡ PCG Overridable

</details>

### Derived Operations

* Box Fitting - Uses axis-aligned bounding boxes
* Box Fitting v2 - Advanced box fitting with configurable extents
* Radius Fitting - Uses spherical collision detection

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExFittingRelaxBase.h)
