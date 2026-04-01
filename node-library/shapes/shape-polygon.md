---
description: Create points as a regular polygon or star.
icon: circle-dashed
---

# Shape : Polygon

### Overview

This shape builder generates points arranged as a regular polygon (triangle, square, pentagon, hexagon, etc.) with configurable vertex count. It can optionally include a skeleton structure with spokes connecting the center to vertices or edge midpoints. The shape can be oriented with a vertex or edge facing forward, and various attributes can be written to track point positions within the polygon.

### How It Works

1. **Calculate Geometry**: Computes vertex positions for regular polygon.
2. **Add Skeleton**: Optionally creates center-to-vertex/edge connections.
3. **Distribute Points**: Places points along edges based on resolution.
4. **Write Attributes**: Outputs hull status, angle, and edge data.

**Usage Notes**

* **Vertex Count**: Controls polygon type (3=triangle, 4=square, 5=pentagon, etc.).
* **Skeleton**: Adds spokes from center to vertices, edges, or both.
* **Orientation**: Align with vertex or edge facing forward.
* **Closed Loop**: Can be flagged as closed for path operations.

### Behavior

**Polygon Generation:**

```
NumVertices = 6 (hexagon), Resolution = 3:

Without skeleton:
   → 6 vertices + 2 points per edge = 18 points on perimeter

With skeleton (Vertex mode):
   → 18 perimeter points + 6 spoke lines to center

With skeleton (Both mode):
   → 18 perimeter + 6 vertex spokes + 6 edge spokes
```

### Outputs

| Pin       | Type  | Description                   |
| --------- | ----- | ----------------------------- |
| **Shape** | Shape | Polygon shape builder factory |

### Settings

#### Polygon Configuration

<details>

<summary><strong>Num Vertices Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the vertex count value.

| Option        | Description               |
| ------------- | ------------------------- |
| **Constant**  | Use fixed vertex count    |
| **Attribute** | Read from point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Number of Vertices</strong> <code>int32</code></summary>

Number of vertices in the polygon (3=triangle, 4=square, 5=pentagon, etc.).

Default: `5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Polygon Orientation</strong> <code>EPCGExPolygonFittingMethod</code></summary>

How the polygon is oriented within its bounds.

| Option             | Description                                 |
| ------------------ | ------------------------------------------- |
| **Vertex Forward** | First vertex faces along local X axis       |
| **Edge Forward**   | First edge is perpendicular to local X axis |
| **Custom**         | Use a custom rotation angle                 |

Default: `Vertex Forward`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Custom Polygon Orientation</strong> <code>float</code></summary>

Custom rotation angle when Polygon Orientation is set to Custom.

Default: `0`

📋 _Visible when Polygon Orientation = Custom_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Is Closed Loop</strong> <code>bool</code></summary>

When enabled, flags the polygon as a closed loop for path operations.

Default: `true`

⚡ PCG Overridable

</details>

#### Skeleton

<details>

<summary><strong>Add Skeleton Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the skeleton toggle.

| Option        | Description               |
| ------------- | ------------------------- |
| **Constant**  | Use fixed boolean         |
| **Attribute** | Read from point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Add Skeleton</strong> <code>bool</code></summary>

When enabled, adds spoke lines from the center to the polygon perimeter.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Skeleton Connection Mode</strong> <code>EPCGExPolygonSkeletonConnectionType</code></summary>

Where skeleton spokes connect to the polygon.

| Option     | Description                        |
| ---------- | ---------------------------------- |
| **Vertex** | Connect to each vertex             |
| **Edge**   | Connect to each edge midpoint      |
| **Both**   | Connect to both vertices and edges |

Default: `Vertex`

📋 _Visible when Add Skeleton is enabled_

</details>

#### Output Attributes

<details>

<summary><strong>Write Hull Attribute</strong> <code>bool</code></summary>

When enabled, writes a boolean indicating if point is on the polygon perimeter.

Default: `false`

</details>

<details>

<summary><strong>On Hull Attribute</strong> <code>FName</code></summary>

Name of the hull boolean attribute.

Default: `bIsOnHull`

📋 _Visible when Write Hull Attribute is enabled_

</details>

<details>

<summary><strong>Write Angle Attribute</strong> <code>bool</code></summary>

When enabled, writes the angle of each point relative to the center.

Default: `false`

</details>

<details>

<summary><strong>Angle Attribute</strong> <code>FName</code></summary>

Name of the angle attribute.

Default: `Angle`

📋 _Visible when Write Angle Attribute is enabled_

</details>

<details>

<summary><strong>Write Edge Index Attribute</strong> <code>bool</code></summary>

When enabled, writes which edge each point belongs to.

Default: `false`

</details>

<details>

<summary><strong>Edge Index Attribute</strong> <code>FName</code></summary>

Name of the edge index attribute.

Default: `EdgeIndex`

📋 _Visible when Write Edge Index Attribute is enabled_

</details>

<details>

<summary><strong>Write Edge Alpha Attribute</strong> <code>bool</code></summary>

When enabled, writes the position along the edge (0-1).

Default: `false`

</details>

<details>

<summary><strong>Edge Alpha Attribute</strong> <code>FName</code></summary>

Name of the edge alpha attribute.

Default: `Alpha`

📋 _Visible when Write Edge Alpha Attribute is enabled_

</details>

#### Shape Settings (Inherited)

<details>

<summary><strong>Resolution Mode</strong> <code>EPCGExResolutionMode</code></summary>

How resolution is interpreted.

| Option       | Description                    |
| ------------ | ------------------------------ |
| **Fixed**    | Points per edge                |
| **Distance** | Target distance between points |

Default: `Fixed`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Resolution</strong> <code>double</code></summary>

Points per edge (Fixed mode) or distance between points (Distance mode).

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fitting</strong> <code>FPCGExFittingDetailsHandler</code></summary>

Controls how the polygon fits within the seed point's bounds.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Points Look At</strong> <code>EPCGExShapePointLookAt</code></summary>

Orientation mode for generated points.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Extents</strong> <code>FVector</code></summary>

Default polygon size when seed has no scale information.

Default: `(50, 50, 50)`

⚡ PCG Overridable

</details>

#### Pruning (Inherited)

<details>

<summary><strong>Remove Below</strong> <code>bool</code></summary>

Discard shapes with fewer points than minimum.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Point Count</strong> <code>int32</code></summary>

Minimum points required.

Default: `2`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remove Above</strong> <code>bool</code></summary>

Discard shapes with more points than maximum.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Point Count</strong> <code>int32</code></summary>

Maximum points allowed.

Default: `500`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsShapes-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsShapes/Public/Shapes/PCGExShapePolygon.h)
