---
description: A more flexible alternative to the native Spawn Dynamic Mesh.
icon: circle
---

# Spawn Dynamic Mesh

### Overview

This node spawns PCG Dynamic Mesh data as Dynamic Mesh Components in the world. It provides extended control over component properties, attachment rules, and post-processing compared to the built-in PCG spawn node.

### How It Works

1. **Receive Mesh Data**: Takes PCG Dynamic Mesh data from upstream nodes (such as topology processors or Clipper2 triangulation).
2. **Configure Component**: Applies the template descriptor settings to configure rendering, collision, shadows, and other component properties.
3. **Spawn Component**: Creates a Dynamic Mesh Component attached to the target actor using the specified attachment rules.
4. **Apply Overrides**: Property overrides are applied to customize individual instances.
5. **Post-Process**: Optionally calls specified functions on the target actor after spawning.

**Usage Notes**

* **Target Actor**: If no target actor is specified, components are attached to the PCG component's owning actor.
* **Property Overrides**: Use property override descriptions to vary settings per-instance based on attributes or other criteria.
* **Post-Process Functions**: Functions must be parameter-less and have the "CallInEditor" flag enabled in their UFUNCTION declaration.

### Inputs

| Pin    | Type             | Description                              |
| ------ | ---------------- | ---------------------------------------- |
| **In** | PCG Dynamic Mesh | Dynamic mesh data to spawn as components |

### Outputs

| Pin     | Type             | Description                         |
| ------- | ---------------- | ----------------------------------- |
| **Out** | PCG Dynamic Mesh | Pass-through of the input mesh data |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Template Descriptor</strong> <code>FPCGExDynamicMeshDescriptor</code></summary>

Comprehensive settings for the spawned Dynamic Mesh Component. This descriptor includes:

**Visibility & LOD**

* `Visible` - Whether the component is visible. Default: `true`
* `Min Draw Distance` - Minimum distance before the mesh is rendered. Default: `0`
* `Desired Max Draw Distance` - Maximum draw distance. Default: `0` (unlimited)

**Lighting**

* `Indirect Lighting Cache Quality` - Quality of indirect lighting cache
* `Lightmap Type` - Type of lightmap to use. Default: `Default`
* `Cast Shadow` - Whether to cast shadows
* `Emissive Light Source` - Whether to act as an emissive light source
* Various shadow casting options (dynamic, static, far, inset, contact, etc.)

**Collision**

* `Body Instance` - Physics body configuration
* `Enable Complex Collision` - Use complex collision geometry. Default: `false`
* `Defer Collision Updates` - Delay collision updates for performance. Default: `false`

**Rendering**

* `Render in Main Pass` - Include in main rendering pass
* `Render in Depth Pass` - Include in depth pass
* `Receives Decals` - Whether decals project onto this mesh
* `Override Materials` - Material overrides for the mesh
* `Overlay Material` - Additional overlay material

**Dynamic Mesh Specific**

* `Use Async Cooking` - Cook mesh data asynchronously. Default: `false`
* `Wireframe Overlay` - Show wireframe overlay. Default: `false`
* `Wireframe Color` - Color for wireframe. Default: `(0, 0.5, 1)`
* `Color Override` - Vertex color override mode. Default: `None`
* `Constant Color` - Color when using constant override. Default: `White`
* `Vertex Color Space` - Color space transform for vertex colors. Default: `NoTransform`
* `Flat Shading` - Use flat shading. Default: `false`
* `Enable Raytracing` - Include in raytracing. Default: `true`

⚡ PCG Overridable (select properties)

</details>

<details>

<summary><strong>Target Actor</strong> <code>TSoftObjectPtr&#x3C;AActor></code></summary>

The actor to attach spawned components to. If not specified, uses the PCG component's owning actor.

Default: None (uses owning actor)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Property Override Descriptions</strong> <code>TArray&#x3C;FPCGObjectPropertyOverrideDescription></code></summary>

List of property overrides to apply to spawned components. Allows customizing individual instances based on input data.

Default: Empty

</details>

<details>

<summary><strong>Attachment Rules</strong> <code>FPCGExAttachmentRules</code></summary>

Rules for how components attach to their parent actor.

Default: All rules set to `KeepRelative`

//→ See TODO FPCGExAttachmentRules

</details>

<details>

<summary><strong>Post Process Function Names</strong> <code>TArray&#x3C;FName></code></summary>

List of function names to call on the target actor after components are spawned. Functions must be:

* Parameter-less
* Have the `CallInEditor` UFUNCTION flag enabled

Default: Empty

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTopology-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTopology/Public/Elements/PCGExSpawnDynamicMesh.h)
