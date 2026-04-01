---
description: >-
  Configures how objects are aligned and positioned relative to their bounds on
  each axis.
icon: sliders-simple
---

# Justification Details

### Overview

This settings block controls justification — the process of aligning an object's pivot point relative to its bounds and a target position. Each axis can be configured independently, allowing you to anchor objects from their center, edges, or custom positions. This is essential for precise placement when staging assets or fitting meshes to paths.

### How It Works

1. **Select Axes**: Enable justification for X, Y, and/or Z axes
2. **Set From Point**: Define where on the object to anchor (center, min, max, or custom)
3. **Set To Point**: Define where in the target space to place that anchor
4. **Apply Offset**: Object is repositioned so its "from" point aligns with the "to" point

### Behavior

```
Justification Example (one axis):

Object Bounds: [----●----]  (● = center)
               Min  ↑  Max

From: Center, To: Min
Result: [----●----]
        ↑ Object center now at target min

From: Min, To: Center
Result:        [----●----]
               ↑ Object min now at target center

From: Max, To: Max
Result:              [----●----]
                            ↑ Object max at target max
```

### Settings

<details>

<summary><strong>Do Justify X</strong> <code>bool</code></summary>

Enables justification on the X axis.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Justify X</strong> <code>FPCGExSingleJustifyDetails</code></summary>

Justification settings for the X axis. See Single Justify Details below.

📋 _Visible when Do Justify X = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Do Justify Y</strong> <code>bool</code></summary>

Enables justification on the Y axis.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Justify Y</strong> <code>FPCGExSingleJustifyDetails</code></summary>

Justification settings for the Y axis. See Single Justify Details below.

📋 _Visible when Do Justify Y = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Do Justify Z</strong> <code>bool</code></summary>

Enables justification on the Z axis.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Justify Z</strong> <code>FPCGExSingleJustifyDetails</code></summary>

Justification settings for the Z axis. See Single Justify Details below.

📋 _Visible when Do Justify Z = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Shared Custom From Attribute</strong> <code>bool</code></summary>

When enabled, uses a single vector attribute to drive custom "from" values for all axes instead of per-axis settings.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom From Vector Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The vector attribute providing custom "from" positions for all axes.

📋 _Visible when Shared Custom From Attribute = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Shared Custom To Attribute</strong> <code>bool</code></summary>

When enabled, uses a single vector attribute to drive custom "to" values for all axes instead of per-axis settings.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom To Vector Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The vector attribute providing custom "to" positions for all axes.

📋 _Visible when Shared Custom To Attribute = true_

⚡ PCG Overridable

</details>

#### Single Justify Details

Each axis uses `FPCGExSingleJustifyDetails` with the following options:

| Setting  | Description                                                                                 |
| -------- | ------------------------------------------------------------------------------------------- |
| **From** | Where on the object bounds to anchor: Center, Min, Max, or Custom (0-1 range)               |
| **To**   | Where in target space to place the anchor: Same (matches From), Center, Min, Max, or Custom |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Fitting/PCGExFitting.h)
