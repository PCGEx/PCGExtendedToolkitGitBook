---
description: PCG mesh selector for spawning from staged collection data.
icon: function
---

# Staging Data Mesh Selector

### Overview

This mesh selector integrates with PCG's native Static Mesh Spawner node to spawn meshes from staged collection data. It reads collection map references from points (created by the Asset Staging node) and uses that information to spawn the appropriate meshes with materials, descriptors, and other configuration from the collections.

### How It Works

1. **Reference Reading**: Reads collection map references from point attributes
2. **Entry Retrieval**: Looks up the corresponding collection entry for each point
3. **Mesh Configuration**: Applies mesh, materials, and component descriptors from the collection
4. **Instance Creation**: Creates mesh instances using PCG's spawning system
5. **Descriptor Application**: Applies either collection descriptors or a template descriptor to spawned instances

**Usage Notes**

* **PCG Integration**: This is a mesh selector for use with PCG's Static Mesh Spawner node, not a standalone PCGEx node
* **Staging Dependency**: Requires points to be staged first using Asset Staging node with Collection Map output mode
* **Template Override**: Use Template Descriptor to override collection descriptor settings with a common configuration
* **Material Variants**: Automatically applies material variant selections from staging data

### Usage

In PCG's Static Mesh Spawner node:

1. Set Mesh Selector Type to **PCGEx | Staging Data**
2. Configure the selector settings
3. Connect staged point data as input
4. Connect collection map(s) to the override pin

### Settings

<details>

<summary><strong>Apply Material Overrides</strong> <code>bool</code></summary>

Apply material override selections from the collection. When enabled, uses material variant picks from staging data.

Default: `true`

</details>

<details>

<summary><strong>Force Disable Collisions</strong> <code>bool</code></summary>

Override all collision settings and disable collisions on all spawned mesh instances.

Default: `false`

</details>

<details>

<summary><strong>Use Template Descriptor</strong> <code>bool</code></summary>

Use a template ISM component descriptor instead of the descriptors defined in the collection. When enabled, only mesh, materials, and tags are pulled from the collection - all other component settings come from the template.

Default: `true`

</details>

<details>

<summary><strong>Template Descriptor</strong> <code>FPCGSoftISMComponentDescriptor</code></summary>

The ISM component descriptor to use when Use Template Descriptor is enabled. Defines rendering, collision, physics, and other component properties for spawned instances.

Default: Empty descriptor

📋 _Visible when Use Template Descriptor is enabled_

</details>

<details>

<summary><strong>Use Time Slicing</strong> <code>bool</code></summary>

Enable time slicing for mesh instance creation to improve editor responsiveness when spawning large numbers of instances.

Default: `false`

</details>

<details>

<summary><strong>Output Points</strong> <code>bool</code></summary>

Whether to output points through the Static Mesh Spawner node. When disabled, only mesh instances are created.

Default: `true`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/MeshSelectors/PCGExMeshSelectorStaged.h)
