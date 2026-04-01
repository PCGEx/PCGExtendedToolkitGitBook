---
description: >-
  Find the collision point on the nearest collidable surface in a given
  direction.
icon: circle
---

# Sample : Line Trace

### Overview

This node performs directional line traces from each point to find collision surfaces. Unlike Nearest Surface which searches in all directions, this node traces along a specified direction from a configurable origin point. It can output extensive hit data including UV coordinates, vertex colors, face indices, and material information.

### How It Works

1. **Configure Trace**: Sets origin point and trace direction from attributes.
2. **Cast Line Trace**: Performs collision trace along the direction.
3. **Process Hit**: Extracts hit location, normal, and surface data.
4. **Write Results**: Outputs hit information to point attributes.

**Usage Notes**

* **Directional**: Traces along a specific direction, not omnidirectional.
* **Origin**: Trace can start from point position or a custom origin attribute.
* **UV Sampling**: Requires complex traces and project settings enabled.
* **Material Info**: Can extract physical and render material data.
* **Vertex Color**: Supports extraction of vertex color at hit location.

### Behavior

**Line Trace Sampling:**

```
Point P with:
   Origin: $Position
   Direction: $Rotation.Down (pointing at ground)
   MaxDistance: 500

Trace Result:
   Hits terrain at distance 200
   → Location = hit point on terrain
   → Normal = terrain surface normal
   → Distance = 200
   → UVCoords = (0.3, 0.7) if complex trace
```

### Inputs

| Pin                  | Type   | Description                                     |
| -------------------- | ------ | ----------------------------------------------- |
| **In**               | Points | Source points to trace from                     |
| **Actor References** | Points | Optional points with actor reference attributes |
| **Texture Params**   | Params | Optional texture parameter definitions          |
| **Point Filters**    | Params | Optional filters for source points              |

### Settings

#### Trace Configuration

<details>

<summary><strong>Surface Source</strong> <code>EPCGExSurfaceSource</code></summary>

Which surfaces to consider for tracing.

| Option               | Description                          |
| -------------------- | ------------------------------------ |
| **Any Surface**      | Trace against any collidable surface |
| **Actor References** | Only trace against specified actors  |

Default: `Actor References`

</details>

<details>

<summary><strong>Origin</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute or property for the trace origin point.

Default: Point position

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute or property for the trace direction vector.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, inverts the trace direction.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Distance Input</strong> <code>EPCGExTraceSampleDistanceInput</code></summary>

Source for the trace distance.

| Option               | Description                          |
| -------------------- | ------------------------------------ |
| **Direction Length** | Use the direction vector's magnitude |
| **Constant**         | Use fixed MaxDistance value          |
| **Attribute**        | Read distance from attribute         |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Distance</strong> <code>double</code></summary>

Maximum trace distance when using Constant mode.

Default: `1000`

📋 _Visible when Distance Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Apply Sampling</strong> <code>FPCGExApplySamplingDetails</code></summary>

Whether to apply hit transform directly to source points.

</details>

<details>

<summary><strong>Rotation Construction</strong> <code>EPCGExMakeRotAxis</code></summary>

How hit transform rotation is constructed from the impact normal.

Default: `Z`

⚡ PCG Overridable

</details>

#### Collision Settings

<details>

<summary><strong>Collision Settings</strong> <code>FPCGExCollisionDetails</code></summary>

Configures collision trace parameters including channels, profiles, and complex tracing.

⚡ PCG Overridable

</details>

#### Outputs

<details>

<summary><strong>Write Success</strong> <code>bool</code></summary>

Writes trace success to a boolean attribute.

Default: `false`

</details>

<details>

<summary><strong>Write Location</strong> <code>bool</code></summary>

Writes the hit location as a vector.

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

Writes the distance to the hit point.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Is Inside</strong> <code>bool</code></summary>

Writes whether the point started inside geometry.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write UV Coords</strong> <code>bool</code></summary>

Writes UV coordinates at the hit point. Requires complex traces and project settings.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Face Index</strong> <code>bool</code></summary>

Writes the index of the hit triangle. Requires complex traces.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Vertex Color</strong> <code>bool</code></summary>

Computes and writes vertex color at hit point to $Color.

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

<summary><strong>Write Hit Component</strong> <code>bool</code></summary>

Writes a reference to the hit component.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Phys Mat</strong> <code>bool</code></summary>

Writes the physical material of the hit surface.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Render Mat</strong> <code>bool</code></summary>

Writes the render material of the hit surface.

Default: `false`

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Successes</strong> <code>bool</code></summary>

Adds a tag if any trace succeeded.

Default: `false`

</details>

<details>

<summary><strong>Tag If Has No Successes</strong> <code>bool</code></summary>

Adds a tag if all traces failed.

Default: `false`

</details>

### Outputs

| Pin     | Type   | Description                |
| ------- | ------ | -------------------------- |
| **Out** | Points | Points with trace hit data |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleSurfaceGuided.h)
