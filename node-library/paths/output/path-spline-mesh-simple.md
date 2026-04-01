---
description: Create spline mesh components from paths.
icon: circle
---

# Path : Spline Mesh (Simple)

### Overview

This node generates Unreal Spline Mesh components along path segments, creating smooth deformable meshes that follow the path curvature. Each segment between consecutive path points becomes a spline mesh instance with configurable tangents, offsets, and mesh properties.

{% hint style="info" %}
Can read per-point meshes and materials from attributes
{% endhint %}

### How It Works

1. **Load Assets**: Load the static mesh (and optionally material) from constant or attribute values.
2. **Process Segments**: For each segment between consecutive path points, prepare spline mesh data.
3. **Compute Tangents**: Calculate or read tangent vectors for smooth curve interpolation.
4. **Apply Mutations**: Apply start/end offsets and expansion settings.
5. **Spawn Components**: Create spline mesh components attached to the target actor.

**Usage Notes**

* **Asset Selection**: Meshes can be specified as a constant or read from a string attribute containing the asset path.
* **Tangents**: For smooth curves, configure tangent settings. Use the Tangents factory input for advanced control or read from pre-computed attributes.
* **Up Vector**: The spline mesh up vector controls how the mesh orients around the spline. Use Constant for uniform orientation, Attribute for per-point control, or Tangents to derive from the path geometry.
* **Component Properties**: Use the Static Mesh Descriptor to configure rendering, collision, and other component settings.

### Inputs

| Pin         | Type             | Description                                          |
| ----------- | ---------------- | ---------------------------------------------------- |
| **In**      | Points           | Path points defining spline mesh segments            |
| **Filters** | Filter Factories | Filters for which points create spline mesh segments |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Asset Type</strong> <code>EPCGExInputValueType</code></summary>

How the static mesh asset is selected.

| Option        | Description                                       |
| ------------- | ------------------------------------------------- |
| **Constant**  | Use a single static mesh for all segments.        |
| **Attribute** | Read the mesh asset path from a string attribute. |

Default: `Attribute`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Asset</strong> <code>TSoftObjectPtr&#x3C;UStaticMesh></code></summary>

The static mesh to use for spline mesh components.

📋 _Visible when Asset Type = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Read Material From Attribute</strong> <code>bool</code></summary>

Read a material override from a string attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Material Attribute Name</strong> <code>FName</code></summary>

Attribute containing the material asset path.

Default: `MaterialPath`

📋 _Visible when Read Material From Attribute = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Slot</strong> <code>int32</code></summary>

Material slot index to apply the material to.

Default: `0`

📋 _Visible when Read Material From Attribute = true_

⚡ PCG Overridable

</details>

***

#### Target Actor

<details>

<summary><strong>Target Actor</strong> <code>TSoftObjectPtr&#x3C;AActor></code></summary>

The actor to attach spline mesh components to. If not set, uses the PCG component's owning actor.

⚡ PCG Overridable

</details>

***

#### Tangents

<details>

<summary><strong>Tangents</strong> <code>FPCGExTangentsDetails</code></summary>

Configuration for spline mesh tangent vectors.

| Property                     | Description                                                 |
| ---------------------------- | ----------------------------------------------------------- |
| **Source**                   | Where tangents come from (Attribute, Factory).              |
| **Arrive Tangent Attribute** | Attribute name for arrive tangent. Default: `ArriveTangent` |
| **Leave Tangent Attribute**  | Attribute name for leave tangent. Default: `LeaveTangent`   |
| **Tangents**                 | Factory for computing tangents.                             |
| **Scaling**                  | Tangent length scaling options.                             |

⚡ PCG Overridable

</details>

***

#### Mutations

<details>

<summary><strong>Start Offset</strong> <code>FVector2D</code></summary>

2D offset applied to the start of each spline mesh segment.

Default: `(0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>End Offset</strong> <code>FVector2D</code></summary>

2D offset applied to the end of each spline mesh segment.

Default: `(0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Expansion</strong> <code>FPCGExSplineMeshMutationDetails</code></summary>

Settings for pushing/expanding the start and end of each segment.

| Property              | Description                                     |
| --------------------- | ----------------------------------------------- |
| **Push Start**        | Enable start point expansion.                   |
| **Start Push Amount** | How much to push the start.                     |
| **Push End**          | Enable end point expansion.                     |
| **End Push Amount**   | How much to push the end.                       |
| **Relative**          | Whether amounts are relative to segment length. |

⚡ PCG Overridable

</details>

***

#### Up Vector

<details>

<summary><strong>Spline Mesh Up Mode</strong> <code>EPCGExSplineMeshUpMode</code></summary>

How the spline mesh up vector is determined.

| Option        | Description                                  |
| ------------- | -------------------------------------------- |
| **Constant**  | Use a single up vector for all segments.     |
| **Attribute** | Read up vector from a point attribute.       |
| **Tangents**  | Derive up vector from path tangent geometry. |

Default: `Constant`

</details>

<details>

<summary><strong>Spline Mesh Up Vector</strong> <code>FVector</code></summary>

Constant up vector for spline mesh orientation.

Default: `(0, 0, 1)` (World Up)

📋 _Visible when Spline Mesh Up Mode = Constant_

⚡ PCG Overridable

</details>

***

#### Component Settings

<details>

<summary><strong>Static Mesh Descriptor</strong> <code>FPCGExStaticMeshComponentDescriptor</code></summary>

Detailed configuration for the spawned spline mesh components, including:

* Visibility and draw distance settings
* Lighting and shadow options
* Collision and physics settings
* LOD configuration
* Material overrides

</details>

<details>

<summary><strong>Property Override Descriptions</strong> <code>TArray</code></summary>

Additional property overrides to apply to spawned components.

</details>

<details>

<summary><strong>Post Process Function Names</strong> <code>TArray&#x3C;FName></code></summary>

Functions to call on the target actor after spline mesh creation. Functions must be parameter-less with "CallInEditor" flag enabled.

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathSplineMeshSimple.h)
