---
description: Configures mesh generation settings for topology operations.
icon: sliders-simple
---

# Topology Details

### Overview

This settings block controls how dynamic meshes are generated from point data, paths, or clusters. It provides configuration for materials, vertex colors, UV channels, triangulation options, normal computation, and edge welding. These settings are shared across topology nodes that convert PCG geometry into renderable meshes.

### How It Works

1. **Collect Geometry**: Gather points/paths to convert to mesh
2. **Triangulate**: Convert polygons to triangles using specified options
3. **Generate UVs**: Create UV coordinates for texturing
4. **Compute Normals**: Calculate surface normals for lighting
5. **Weld Edges**: Optionally merge coincident edges
6. **Apply Material**: Assign material and vertex colors

### Behavior

```
Topology Pipeline:

Points/Paths → Triangulation → UV Generation → Normals → Mesh
     │              │               │            │
     ▼              ▼               ▼            ▼
  Vertices      Triangles      Texture       Lighting
  Positions      Indices      Coordinates    Vectors

Output: Dynamic Mesh Component with material applied
```

### Settings

#### Material & Color

<details>

<summary><strong>Material</strong> <code>TSoftObjectPtr&#x3C;UMaterialInterface></code></summary>

The material to apply to the generated mesh.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Vertex Color</strong> <code>FLinearColor</code></summary>

The default color applied to mesh vertices. Can be used with vertex-color-enabled materials.

Default: `White`

⚡ PCG Overridable

</details>

#### UV & Triangulation

<details>

<summary><strong>UV Channels</strong> <code>FPCGExTopologyUVDetails</code></summary>

Configuration for UV channel generation. Supports multiple UV channels with different projection methods.

</details>

<details>

<summary><strong>Primitive Options</strong> <code>FGeometryScriptPrimitiveOptions</code></summary>

Options for primitive geometry generation from the Geometry Script system.

</details>

<details>

<summary><strong>Triangulation Options</strong> <code>FGeometryScriptPolygonsTriangulationOptions</code></summary>

Options controlling how polygons are triangulated, including handling of complex or self-intersecting polygons.

</details>

<details>

<summary><strong>Quiet Triangulation Error</strong> <code>bool</code></summary>

When enabled, suppresses warning messages when triangulation encounters errors.

Default: `false`

⚡ PCG Overridable

</details>

#### Mesh Processing

<details>

<summary><strong>Weld Edges</strong> <code>bool</code></summary>

When enabled, merges coincident edges to create a seamless mesh.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weld Edges Options</strong> <code>FGeometryScriptWeldEdgesOptions</code></summary>

Options controlling edge welding tolerance and behavior.

📋 _Visible when Weld Edges = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compute Normals</strong> <code>bool</code></summary>

When enabled, calculates surface normals for proper lighting.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Normals Options</strong> <code>FGeometryScriptCalculateNormalsOptions</code></summary>

Options for normal calculation, including smoothing angles and weighting.

📋 _Visible when Compute Normals = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flip Normals</strong> <code>bool</code></summary>

When enabled, reverses the direction of all surface normals.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Coordinate Space</strong> <code>EPCGCoordinateSpace</code></summary>

The coordinate space for the output mesh geometry.

Default: `OriginalComponent`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTopology-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTopology/Public/PCGExTopology.h)
