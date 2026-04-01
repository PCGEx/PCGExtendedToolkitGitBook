---
description: Filters points based on raycast results against surfaces.
icon: circle-dashed
---

# Filter : Raycast

### Overview

This filter casts rays from each point and tests whether they hit geometry. Points pass or fail based on hit detection or by comparing hit distance against a threshold. The filter can trace against all scene geometry or only specific actors referenced by attribute.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Determine Origin**: For each point, calculate the ray origin based on the origin mode (point position, offset, or world position).
2. **Calculate Direction**: Get the ray direction from the configured source, optionally transforming it by the point's rotation.
3. **Perform Raycast**: Cast a ray using the collision settings (line trace, sphere sweep, or box sweep) up to the maximum distance.
4. **Evaluate Result**: In "Any Hit" mode, pass if the ray hits anything. In "Compare Distance" mode, compare the hit distance against the threshold using the specified comparison operator.

**Usage Notes**

* **Actor References**: When using the "Actor References" surface source, you typically get actor paths from a Get Actor Data PCG node in point mode.
* **No Hit Handling**: In Compare Distance mode, the "No Hit Fallback" setting determines whether points without hits pass or fail, since there's no distance to compare.

### Behavior

```
Point                Ray                    Surface
  ●─────────────────────────────────────────▓▓▓
  │                                          │
  │           distance = 850                 │
  │                                          │

Test Mode: Any Hit
→ PASS (hit detected)

Test Mode: Compare Distance (< 1000)
→ PASS (850 < 1000)

Test Mode: Compare Distance (< 500)
→ FAIL (850 >= 500)
```

### Settings

#### Surface Source

<details>

<summary><strong>Surface Source</strong> <code>EPCGExSurfaceSource</code></summary>

Whether to trace against all scene geometry or only specific actors.

| Option               | Description                                      |
| -------------------- | ------------------------------------------------ |
| **All**              | Trace against any surface in the scene           |
| **Actor References** | Only trace against actors specified by attribute |

Default: `All`

</details>

<details>

<summary><strong>Actor Reference</strong> <code>FName</code></summary>

Name of the attribute containing actor paths (usually from Get Actor Data node).

Default: `ActorReference`

📋 _Visible when Surface Source = Actor References_

⚡ PCG Overridable

</details>

#### Test Mode

<details>

<summary><strong>Test Mode</strong> <code>EPCGExRaycastTestMode</code></summary>

How to evaluate the raycast result.

| Option               | Description                                  |
| -------------------- | -------------------------------------------- |
| **Any Hit**          | Pass if the ray hits anything                |
| **Compare Distance** | Compare the hit distance against a threshold |

Default: `Any Hit`

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator for distance comparison.

Default: `Strictly Smaller`

📋 _Visible when Test Mode = Compare Distance_

</details>

<details>

<summary><strong>Distance Threshold</strong> <code>double</code></summary>

The distance value to compare against.

Default: `500.0`

📋 _Visible when Test Mode = Compare Distance_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance for nearly equal/not equal comparisons.

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Test Mode = Compare Distance and Comparison = Nearly Equal or Nearly Not Equal_

⚡ PCG Overridable

</details>

<details>

<summary><strong>No Hit Fallback</strong> <code>EPCGExFilterFallback</code></summary>

What result to return when there is no hit in Compare Distance mode, since there's no distance to compare.

| Option   | Description                         |
| -------- | ----------------------------------- |
| **Pass** | Points without hits pass the filter |
| **Fail** | Points without hits fail the filter |

Default: `Fail`

📋 _Visible when Test Mode = Compare Distance_

</details>

#### Ray Settings

<details>

<summary><strong>Origin Mode</strong> <code>EPCGExRaycastOriginMode</code></summary>

How to determine the ray's starting position.

| Option                | Description                                                    |
| --------------------- | -------------------------------------------------------------- |
| **Point Position**    | Use the point's position directly                              |
| **Offset (World)**    | Point position plus offset in world space                      |
| **Offset (Relative)** | Point position plus offset transformed by point rotation/scale |
| **World Position**    | Use offset value as absolute world position                    |

Default: `Point Position`

</details>

<details>

<summary><strong>Origin</strong> <code>FVector</code></summary>

Origin offset or world position depending on mode.

Default: `(0, 0, 0)`

📋 _Visible when Origin Mode is not Point Position_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>FVector</code></summary>

The direction to cast the ray.

Default: `Down (0, 0, -1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform Direction</strong> <code>bool</code></summary>

Whether to transform the ray direction using the point's rotation.

Default: `false`

</details>

<details>

<summary><strong>Max Distance</strong> <code>double</code></summary>

Maximum distance to trace.

Default: `1000.0`

⚡ PCG Overridable

</details>

#### Collision Settings

<details>

<summary><strong>Collision Settings</strong> <code>FPCGExCollisionDetails</code></summary>

Configuration for the trace operation including trace type (line, sphere, box), collision channels, and complex vs simple collision.

→ See Collision Details for full reference.

</details>

#### Filter Settings

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Whether to invert the filter result. Inversion is applied after the test, not to fallback values.

Default: `false`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Filters/Points/PCGExRaycastFilter.h)
