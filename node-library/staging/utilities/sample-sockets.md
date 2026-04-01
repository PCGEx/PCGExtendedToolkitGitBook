---
description: Parse static mesh paths and output sockets as points.
icon: circle
---

# Sample : Sockets

### Overview

This node reads static mesh assets and extracts their socket locations as point data. Each socket becomes a point with the socket's transform, and optionally includes metadata like socket name, tag, and category. This enables procedural placement at mesh socket locations defined in the static mesh editor.

### How It Works

1. **Load Meshes**: Reads static mesh assets from path attribute or constant.
2. **Find Sockets**: Locates all sockets defined on each mesh.
3. **Filter Sockets**: Optionally filters by socket name or tag patterns.
4. **Create Points**: Generates points at each socket's transform.
5. **Write Metadata**: Outputs socket name, tag, and asset path to attributes.

**Usage Notes**

* **Asset Source**: Read mesh path from attribute or use a constant mesh.
* **Socket Filtering**: Filter by name or tag patterns to select specific sockets.
* **Transform Inheritance**: Socket transforms include position, rotation, and scale.
* **Per-Point Output**: Each input point can generate multiple socket points.

### Behavior

**Socket Extraction:**

```
Input point with $AssetPath = "/Game/Meshes/SM_Building":

SM_Building has sockets:
   "Door_01" (tag: "Entrance")
   "Door_02" (tag: "Entrance")
   "Window_01" (tag: "Opening")

With SocketTagFilters = "Entrance":
   → Outputs 2 points: Door_01, Door_02

With no filters:
   → Outputs 3 points (all sockets)
```

### Inputs

| Pin               | Type   | Description                                  |
| ----------------- | ------ | -------------------------------------------- |
| **In**            | Points | Points with static mesh path attributes      |
| **Point Filters** | Params | Optional filters for which points to process |

### Settings

#### Asset Source

<details>

<summary><strong>Asset Type</strong> <code>EPCGExInputValueType</code></summary>

How the static mesh asset is specified.

| Option        | Description                         |
| ------------- | ----------------------------------- |
| **Constant**  | Use a fixed static mesh asset       |
| **Attribute** | Read mesh path from point attribute |

Default: `Attribute`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Asset (Attr)</strong> <code>FName</code></summary>

Attribute containing the static mesh asset path.

Default: `AssetPath`

📋 _Visible when Asset Type = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Asset</strong> <code>TSoftObjectPtr</code></summary>

Constant static mesh to extract sockets from.

📋 _Visible when Asset Type = Constant_

⚡ PCG Overridable

</details>

#### Socket Output

<details>

<summary><strong>Output Socket Details</strong> <code>FPCGExSocketOutputDetails</code></summary>

Controls socket filtering and output attributes.

| Property                | Description                           |
| ----------------------- | ------------------------------------- |
| **Socket Tag Filters**  | Filter sockets by tag patterns        |
| **Socket Name Filters** | Filter sockets by name patterns       |
| **Write Socket Name**   | Output socket name to attribute       |
| **Write Socket Tag**    | Output socket tag to attribute        |
| **Write Category**      | Output socket category to attribute   |
| **Write Asset Path**    | Output source mesh path to attribute  |
| **Transform Scale**     | Which transform components to inherit |
| **Carry Over Settings** | Forward attributes from input points  |

⚡ PCG Overridable

</details>

### Outputs

| Pin         | Type   | Description                |
| ----------- | ------ | -------------------------- |
| **Sockets** | Points | Points at socket locations |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Elements/PCGExSampleSockets.h)
