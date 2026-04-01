---
description: Configures how socket data is filtered and output as point data.
icon: sliders-simple
---

# Socket Output Details

### Overview

This settings block controls the output of socket information when loading or sampling sockets from meshes. It provides filtering by socket name and tag, options to write socket metadata as point attributes, and carry-over settings for inherited data. Sockets are attachment points defined on meshes that can be used for modular assembly or spawn point placement.

### How It Works

1. **Filter Sockets**: Include/exclude sockets by name or tag patterns
2. **Generate Points**: Create point data at socket locations
3. **Write Metadata**: Optionally write socket name, tag, category, and asset path
4. **Apply Transform**: Configure which transform components to apply
5. **Carry Over**: Control attribute/tag inheritance from source data

### Behavior

```
Socket Loading Pipeline:

Mesh Sockets:
  Socket_Door (tag: "Entrance")
  Socket_Window_L (tag: "Window")
  Socket_Window_R (tag: "Window")
  Socket_Debug (tag: "Debug")

Filter: Exclude tag "Debug"
Output:
  ● Socket_Door     [SocketName="Socket_Door", SocketTag="Entrance"]
  ● Socket_Window_L [SocketName="Socket_Window_L", SocketTag="Window"]
  ● Socket_Window_R [SocketName="Socket_Window_R", SocketTag="Window"]
```

### Settings

#### Socket Filtering

<details>

<summary><strong>Socket Tag Filters</strong> <code>FPCGExNameFiltersDetails</code></summary>

Include or exclude sockets based on their tag. Uses pattern matching to filter by socket tags.

→ See Name Filters Details

⚡ PCG Overridable

</details>

<details>

<summary><strong>Socket Name Filters</strong> <code>FPCGExNameFiltersDetails</code></summary>

Include or exclude sockets based on their name. Uses pattern matching to filter by socket names.

→ See Name Filters Details

⚡ PCG Overridable

</details>

#### Metadata Output

<details>

<summary><strong>Write Socket Name</strong> <code>bool</code></summary>

When enabled, writes the socket name to an attribute.

Default: `false` · Attribute: `SocketName`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Socket Tag</strong> <code>bool</code></summary>

When enabled, writes the socket tag to an attribute.

Default: `false` · Attribute: `SocketTag`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Category</strong> <code>bool</code></summary>

When enabled, writes the socket category to an attribute.

Default: `false` · Attribute: `Category`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Asset Path</strong> <code>bool</code></summary>

When enabled, writes the source asset path to an attribute.

Default: `false` · Attribute: `AssetPath`

⚡ PCG Overridable

</details>

#### Transform & Inheritance

<details>

<summary><strong>Transform Scale</strong> <code>bitmask</code></summary>

Selects which scale components (X, Y, Z) from the socket transform are applied to output points.

Default: All components enabled

</details>

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are carried over from source data to socket points.

→ See Carry Over Details

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Details/PCGExSocketOutputDetails.h)
