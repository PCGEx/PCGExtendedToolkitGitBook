---
description: Fuse collinear path points.
icon: circle
---

# Path : Fuse Collinear

### Overview

This node simplifies paths by removing points that lie approximately on a straight line between their neighbors. Points are considered collinear when the angle between incoming and outgoing segments is below a threshold. The result is a simplified path with fewer points that maintains its overall shape.

### How It Works

1. **Analyze Angles**: For each point, calculate the angle between the direction from the previous point and the direction to the next point.
2. **Test Collinearity**: Points where this angle is below the threshold are considered collinear (removable).
3. **Apply Filters**: Points passing the keep filter are preserved regardless of collinearity.
4. **Fuse Points**: Remove collinear points, optionally blending their attributes into the preserved points.
5. **Handle Collocated**: Optionally fuse points that are nearly at the same position.

**Usage Notes**

* **Threshold**: Higher values remove more points (more aggressive simplification). A threshold of 0° only removes perfectly collinear points; 180° removes all interior points.
* **Invert Mode**: When inverted, removes points that are NOT collinear - useful for smoothing sharp corners while keeping straight sections.
* **Keep Filters**: Use filters to protect specific points from removal (e.g., points with certain tags or attributes).

### Behavior

```
Collinear removal (threshold = 10°):

Before:  ●───●───●───●───●     After:  ●───────────────●
              (collinear)              (interior points removed)

Before:  ●───●               After:  ●───●
              \                           \
               ●  (angle > threshold)      ●  (preserved)
```

### Inputs

| Pin                 | Type             | Description                              |
| ------------------- | ---------------- | ---------------------------------------- |
| **In**              | Points           | Path points to simplify                  |
| **Keep Conditions** | Filter Factories | Filters that protect points from removal |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Threshold</strong> <code>double</code></summary>

Angular threshold in degrees for collinearity. Points where the angle between incoming and outgoing directions is less than this value are considered collinear.

* `0°` - Only perfectly straight segments are fused
* `10°` - Slight deviations are tolerated (default)
* `90°` - Removes all but sharp corners
* `180°` - Removes all interior points

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Threshold</strong> <code>bool</code></summary>

When enabled, removes points that are NOT collinear instead of those that are. This creates a smoothing effect - sharp corners are removed while straight sections are preserved.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fuse Collocated</strong> <code>bool</code></summary>

Treat nearly-overlapping points (within Fuse Distance) as collinear and fuse them.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fuse Distance</strong> <code>double</code></summary>

Distance threshold for considering points as collocated (overlapping).

Default: `0.001`

📋 _Visible when Fuse Collocated = true_

</details>

<details>

<summary><strong>Do Blend</strong> <code>bool</code></summary>

Enable attribute blending when fusing points. When enabled, attributes from removed points are blended into the preserved point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

Controls how attributes from fused points are merged together.

📋 _Visible when Do Blend = true_

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Union Details</strong> <code>FPCGExUnionMetadataDetails</code></summary>

Output metadata about fused points.

| Property             | Description                                                            |
| -------------------- | ---------------------------------------------------------------------- |
| **Write Is Union**   | Mark points that absorbed other points. Attribute: `bIsUnion`          |
| **Write Union Size** | Write how many points were fused into this one. Attribute: `UnionSize` |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Invalid Paths From Output</strong> <code>bool</code></summary>

Exclude paths that become invalid (too few points) after fusion from the output.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExFuseCollinear.h)
