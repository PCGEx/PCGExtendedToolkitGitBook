---
description: Create a Delaunay triangulated surface for each input dataset.
icon: circle
---

# Topology : Point Surface

### Overview

This node generates a triangulated mesh surface directly from point data using Delaunay triangulation. Unlike cluster-based topology nodes, it doesn't require edge connectivity - it computes the optimal triangulation from point positions alone.

### How It Works

1. **Project to 2D**: Points are projected onto a 2D plane using the specified projection method.
2. **Delaunay Triangulation**: The algorithm computes a Delaunay triangulation, which maximizes the minimum angle of all triangles for optimal mesh quality.
3. **Build Mesh**: Triangles are assembled into a PCG Dynamic Mesh with the original 3D positions preserved.
4. **Apply Settings**: Material, UV mapping, normals, and other topology settings are applied to the output mesh.

{% hint style="warning" %}
### Performance Consideration

This node uses Unreal's built-in Geometry Script Delaunay algorithm, which is significantly slower than the Delaunator library used by other PCGEx nodes (such as Delaunay 2D). If you only need the triangulation topology and not a Dynamic Mesh output, consider using those alternatives for better performance.
{% endhint %}

**Usage Notes**

* **Point-Based**: This node works directly on point data without requiring cluster/edge information. Every point becomes a vertex in the output mesh.
* **Convex Hull**: The triangulation covers the convex hull of the projected points. Points on the interior create additional triangles.
* **Minimum Points**: Requires at least 3 non-collinear points to produce a valid triangulation.

### Behavior

```
Input: Scattered points         Output: Triangulated mesh

      ●           ●                   ●───────────●
            ●                              /\  /\
                          →              /  \/  \
    ●       ●       ●               ●───●────●───●
          ●                               \ /\ /
      ●       ●                       ●───●──●

All points become vertices connected by Delaunay triangles.
```

### Inputs

| Pin    | Type   | Description               |
| ------ | ------ | ------------------------- |
| **In** | Points | Point data to triangulate |

### Outputs

| Pin      | Type             | Description                   |
| -------- | ---------------- | ----------------------------- |
| **Mesh** | PCG Dynamic Mesh | The triangulated surface mesh |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Settings for projecting 3D points onto a 2D plane for triangulation. The triangulation is computed in 2D, but the resulting mesh uses the original 3D point positions.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

<details>

<summary><strong>Attempt Repair</strong> <code>bool</code></summary>

Attempt to repair degenerate geometry (zero-area triangles, duplicate vertices) after triangulation.

Default: `false`

</details>

<details>

<summary><strong>Repair Degenerate</strong> <code>FGeometryScriptDegenerateTriangleOptions</code></summary>

Options for handling degenerate triangles during repair.

📋 _Visible when Attempt Repair = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Topology</strong> <code>FPCGExTopologyDetails</code></summary>

Mesh generation settings including material, vertex colors, UV channels, normals, and coordinate space.

See Topology Clusters Processor for detailed topology settings documentation.

⚡ PCG Overridable

</details>

***

#### Warnings and Errors

<details>

<summary><strong>Quiet Bad Vertices Warning</strong> <code>bool</code></summary>

Suppress warnings about degenerate or bad vertices in the mesh.

Default: `false`

</details>

#### Inherited Settings

This node inherits point processing settings from its base class.

→ See Points Processor Settings for: Point data handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTopology-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTopology/Public/Elements/PCGExTopologyPointSurface.h)
