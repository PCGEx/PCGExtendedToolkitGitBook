---
description: Shared configuration for collision traces used across multiple nodes.
icon: sliders-simple
---

# Collision Details

### Overview

This struct defines how collision queries are performed — trace shape, collision filtering, and actor exclusion. It appears as a `CollisionSettings` property on any node that performs raycasts or sweeps against world geometry.

### How It Works

1. **Trace Shape**: Choose between line, sphere sweep, or box sweep. Sphere and box sweeps catch geometry within a volume rather than along a single ray.
2. **Collision Filter**: Select which collision responses to query — by channel, object type, or collision profile. Only one mode is active at a time.
3. **Actor Exclusion**: Optionally ignore the PCG graph's own actors and/or a configurable selection of other actors during traces.

**Usage Notes**

* **Shared Struct**: This is not a standalone node. It appears as an embedded settings group on nodes like **Refine : Line Trace** and **Sample : Nearest Surface**.
* **Trace Complex**: Enabling complex tracing uses per-polygon collision instead of simplified bounds, which is more precise but slower.
* **Self Exclusion**: `Ignore Self` is enabled by default to prevent PCG-spawned geometry from blocking its own traces.

### Settings

<details>

<summary><strong>Trace Mode</strong> <code>EPCGExTraceMode</code></summary>

Shape of the collision trace.

| Option     | Description                                          |
| ---------- | ---------------------------------------------------- |
| **Line**   | Simple line trace — a single ray between two points  |
| **Sphere** | Sphere sweep — traces a sphere volume along the path |
| **Box**    | Box sweep — traces a box volume along the path       |

Default: `Line`

</details>

<details>

<summary><strong>Sphere Radius</strong> <code>FPCGExInputShorthandSelectorDoubleAbs</code></summary>

Radius of the sphere for sphere sweep traces. Supports constant or attribute-driven values.

Default: `50.0`

📋 _Visible when Trace Mode = Sphere_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Box Half Extents</strong> <code>FPCGExInputShorthandSelectorVector</code></summary>

Half extents of the box for box sweep traces. Supports constant or attribute-driven values.

Default: `(25.0, 25.0, 25.0)`

📋 _Visible when Trace Mode = Box_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Trace Complex</strong> <code>bool</code></summary>

Trace against complex collision geometry (per-polygon) instead of simplified bounds. More precise but slower.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Collision Type</strong> <code>EPCGExCollisionFilterType</code></summary>

How to filter which geometry the trace interacts with.

| Option          | Description                      |
| --------------- | -------------------------------- |
| **Channel**     | Filter by collision channel      |
| **Object Type** | Filter by object type query      |
| **Profile**     | Filter by collision profile name |

Default: `Channel`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Collision Channel</strong> <code>ECollisionChannel</code></summary>

Collision channel to trace against.

Default: `ECC_WorldDynamic`

📋 _Visible when Collision Type = Channel_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Collision Object Type</strong> <code>int32</code></summary>

Object type query to trace against.

Default: `ObjectTypeQuery1`

📋 _Visible when Collision Type = Object Type_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Collision Profile Name</strong> <code>FName</code></summary>

Collision profile to trace against.

Default: `None`

📋 _Visible when Collision Type = Profile_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore Self</strong> <code>bool</code></summary>

Ignore actors spawned by this PCG graph during traces.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore Actors</strong> <code>bool</code></summary>

Enable to ignore a configurable selection of actors during traces.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignored Actor Selector</strong> <code>FPCGActorSelectorSettings</code></summary>

Actor selection criteria for actors to exclude from collision checks.

📋 _Visible when Ignore Actors is enabled_

⚡ PCG Overridable

</details>

### Used By

| Node                         | Property                                              |
| ---------------------------- | ----------------------------------------------------- |
| **Refine : Line Trace**      | `CollisionSettings`                                   |
| **Sample : Nearest Surface** | `CollisionSettings`                                   |
| **Sample : Line Trace**      | `CollisionSettings`                                   |
| Tensor : Surface             | `CollisionSettings` (via `FPCGExTensorSurfaceConfig`) |
| Raycast Filter               | `CollisionSettings` (via `FPCGExRaycastFilterConfig`) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Details/PCGExCollisionDetails.h)
