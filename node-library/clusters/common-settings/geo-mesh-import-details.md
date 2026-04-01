---
description: Controls which data is imported from a mesh onto the generated cluster points.
icon: sliders-simple
---

# Geo Mesh Import Details

### Overview

Geo Mesh Import Details is a shared settings struct used by mesh-to-cluster nodes to control what additional data — vertex colors and UV coordinates — gets transferred from the source mesh onto the output point data. UV channels are mapped by name, so you can output the same mesh UV channel to multiple attributes or selectively import only the channels you need.

### Settings

<details>

<summary><strong>Import Vertex Color</strong> <code>bool</code></summary>

Import vertex colors from the mesh as point color data. Colors are averaged per merged vertex.

Default: `true`

</details>

<details>

<summary><strong>Import UVs</strong> <code>bool</code></summary>

Import UV coordinates from the mesh as point attributes.

Default: `false`

</details>

<details>

<summary><strong>UV Channels Mapping</strong> <code>TMap&#x3C;FName, int32></code></summary>

A mapping of output attribute names to mesh UV channel indices. Each entry creates a `FVector2D` attribute with the specified name, populated from the corresponding UV channel. You can map the same UV channel to multiple attribute names.

Default: `empty`

📋 _Visible when Import UVs is enabled_

</details>

<details>

<summary><strong>Create Placeholders</strong> <code>bool</code></summary>

If enabled, creates placeholder attributes when a listed UV channel index doesn't exist on the mesh. Useful when downstream nodes expect those attributes to exist, even if the values are invalid.

Default: `false`

📋 _Visible when Import UVs is enabled_

</details>

<details>

<summary><strong>Placeholder Value</strong> <code>FVector2D</code></summary>

The UV value written to placeholder attributes when a channel is missing.

Default: `(0, 0)`

📋 _Visible when Import UVs and Create Placeholders are both enabled_

</details>

### Used By

* Mesh to Clusters — as `ImportDetails`
* Dynamic Mesh to Clusters — as `ImportDetails`

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Data/External/PCGExMeshImportDetails.h)
