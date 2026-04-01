---
description: Find the closest point on the nearest collidable surface.
icon: circle
---

# Sample : Nearest Surface

> Note : Only works with simple colliders.

### Overview

This node performs collision traces from each point to find the nearest collidable surface in the world. It outputs hit locations, normals, distances, and can detect if points are inside geometry. This is useful for snapping points to surfaces, computing distance fields from geometry, or detecting point containment within collision volumes.

### How It Works

1. **Perform Traces**: Casts sphere traces from each point outward.
2. **Find Nearest Hit**: Identifies the closest collision surface.
3. **Extract Data**: Gets hit location, normal, and surface info.
4. **Write Results**: Outputs distances, normals, and actor references.

**Usage Notes**

* **Surface Source**: Sample any surface or specific actor references.
* **Collision Settings**: Configure channels, profiles, and trace complexity.
* **Inside Detection**: Can identify points inside collision volumes.
* **Physical Materials**: Optionally outputs hit surface's physical material.

### Behavior

**Surface Sampling:**

```
Point P with nearby collision surfaces:
   Surface1: static mesh at distance 50
   Surface2: landscape at distance 100
   Surface3: actor reference at distance 30

SurfaceSource = Any:
   → Hits Surface3 (closest)

SurfaceSource = Actor References:
   → Only considers surfaces from actor reference attribute
```

### Inputs

| Pin                  | Type   | Description                                     |
| -------------------- | ------ | ----------------------------------------------- |
| **In**               | Points | Source points to trace from                     |
| **Actor References** | Points | Optional points with actor reference attributes |
| **Point Filters**    | Params | Optional filters for source points              |

### Settings

#### Sampling

<details>

<summary><strong>Surface Source</strong> <code>EPCGExSurfaceSource</code></summary>

Which surfaces to consider for sampling.

| Option               | Description                                |
| -------------------- | ------------------------------------------ |
| **Any Surface**      | Sample any collidable surface in the world |
| **Actor References** | Only sample surfaces from specified actors |

Default: `Actor References`

</details>

<details>

<summary><strong>Actor Reference</strong> <code>FName</code></summary>

Attribute containing actor reference paths when using Actor References mode.

Default: `ActorReference`

📋 _Visible when Surface Source = Actor References_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Distance</strong> <code>double</code></summary>

Maximum trace distance from each point.

Default: `1000`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Local Max Distance</strong> <code>bool</code></summary>

When enabled, reads max distance from a per-point attribute.

Default: `false`

</details>

<details>

<summary><strong>Apply Sampling</strong> <code>FPCGExApplySamplingDetails</code></summary>

Whether to apply sampled position/rotation directly to source points.

</details>

#### Collision Settings

<details>

<summary><strong>Collision Settings</strong> <code>FPCGExCollisionDetails</code></summary>

Configures collision trace parameters.

| Property              | Description                      |
| --------------------- | -------------------------------- |
| **Trace Complex**     | Use complex collision geometry   |
| **Collision Type**    | Channel, Object Type, or Profile |
| **Collision Channel** | Which channel to trace           |
| **Ignore Self**       | Ignore the generating actor      |
| **Ignore Actors**     | Additional actors to ignore      |

⚡ PCG Overridable

</details>

#### Outputs

<details>

<summary><strong>Write Success</strong> <code>bool</code></summary>

Writes sampling success to a boolean attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Location</strong> <code>bool</code></summary>

Writes the hit location as a vector attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Look At</strong> <code>bool</code></summary>

Writes direction from point to hit location.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Normal</strong> <code>bool</code></summary>

Writes the surface normal at the hit point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Distance</strong> <code>bool</code></summary>

Writes the distance to the nearest surface.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Is Inside</strong> <code>bool</code></summary>

Writes whether the point is inside collision geometry.

Default: `false`

⚡ PCG Overridable

</details>

#### Actor Data Outputs

<details>

<summary><strong>Write Actor Reference</strong> <code>bool</code></summary>

Writes a reference to the hit actor.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Phys Mat</strong> <code>bool</code></summary>

Writes the physical material of the hit surface.

Default: `false`

⚡ PCG Overridable

</details>

#### Tagging & Forwarding

<details>

<summary><strong>Attributes Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Forwards attributes from actor reference points to sampled points.

📋 _Visible when Surface Source = Actor References_

//→ See TODO FPCGExForwardDetails

</details>

<details>

<summary><strong>Tag If Has Successes</strong> <code>bool</code></summary>

Adds a tag if sampling succeeded for any point.

Default: `false`

</details>

<details>

<summary><strong>Tag If Has No Successes</strong> <code>bool</code></summary>

Adds a tag if sampling failed for all points.

Default: `false`

</details>

#### Advanced

<details>

<summary><strong>Process Filtered Out As Fails</strong> <code>bool</code></summary>

When enabled, filtered-out points are marked as failed samples.

Default: `true`

</details>

<details>

<summary><strong>Prune Failed Samples</strong> <code>bool</code></summary>

When enabled, removes points that failed to hit any surface.

Default: `false`

</details>

<details>

<summary><strong>Process Inside As Failed Samples</strong> <code>bool</code></summary>

When enabled, points inside geometry are considered failures.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Process Outside As Failed Samples</strong> <code>bool</code></summary>

When enabled, points outside geometry are considered failures.

Default: `false`

⚡ PCG Overridable

</details>

### Outputs

| Pin     | Type   | Description                       |
| ------- | ------ | --------------------------------- |
| **Out** | Points | Points with surface sampling data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleNearestSurface.h)
