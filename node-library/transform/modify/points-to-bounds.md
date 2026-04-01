---
description: Merge points group to a single point representing their bounds.
icon: circle
---

# Points to Bounds

### Overview

Points to Bounds reduces a collection of points into a single representative point whose transform and bounds encompass all input points. It computes the bounding box (axis-aligned or oriented), blends point properties, and optionally writes aggregate data like bounds extents and best-fit plane information. This is useful for creating simplified representations of point groups.

### How It Works

1. **Bounds Computation**: Calculates the overall bounding box of all input points.
2. **Oriented Bounds (Optional)**: If enabled, computes an oriented bounding box that minimizes volume.
3. **Property Blending**: Blends point properties (density, color, etc.) from all input points.
4. **Best-Fit Plane**: Computes a plane that best fits the point distribution.
5. **Output**: Creates a single point with the computed transform and bounds, or writes data to existing points.

**Usage Notes**

* **Output Modes**: "Collapse" creates a single output point; "Write Data" preserves input points and writes computed values to data-domain attributes.
* **Oriented Bounding Box**: Produces tighter bounds for non-axis-aligned point distributions but ignores individual point bounds.

### Behavior

```
Input Points:                  Output (Collapse mode):

    ●  ●                        ┌─────────┐
  ●      ●          →           │    ●    │  Single point at bounds center
    ●  ●                        └─────────┘  with bounds matching all inputs
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Output Oriented Bounding Box</strong> <code>bool</code></summary>

When enabled, computes an oriented bounding box (OBB) that rotates to minimize volume. This produces tighter bounds for rotated point distributions but only considers point positions, not individual point bounds.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Precise Fit</strong> <code>bool</code></summary>

When enabled, uses a more precise algorithm to find the minimum-volume oriented bounding box.

Default: `true`

📋 _Visible when Output Oriented Bounding Box is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Axis Order</strong> <code>EPCGExAxisOrder</code></summary>

Controls how the oriented bounding box axes are assigned to X, Y, Z based on their lengths.

| Option  | Description                                |
| ------- | ------------------------------------------ |
| **XYZ** | Longest axis → X, middle → Y, shortest → Z |
| **YZX** | Longest axis → Y, middle → Z, shortest → X |
| ...     | Other permutations available               |

Default: `XYZ`

📋 _Visible when Output Oriented Bounding Box is enabled_

</details>

<details>

<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which point property to use when computing the overall bounds.

| Option             | Description                          |
| ------------------ | ------------------------------------ |
| **Scaled Bounds**  | Use point bounds multiplied by scale |
| **Density Bounds** | Use density-based bounds             |
| **Bounds**         | Use raw point bounds                 |
| **Center**         | Use point center positions only      |

Default: `ScaledBounds`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Mode</strong> <code>EPCGExPointsToBoundsOutputMode</code></summary>

How to output the computed bounds data.

| Option         | Description                                                          |
| -------------- | -------------------------------------------------------------------- |
| **Collapse**   | Create a single point representing the combined bounds               |
| **Write Data** | Keep original points and write bounds data to data-domain attributes |

Default: `Collapse`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Properties</strong> <code>bool</code></summary>

When enabled, blends point properties from all input points into the output.

Default: `true`

</details>

<details>

<summary><strong>Blending Settings</strong> <code>FPCGExBlendingDetails</code></summary>

Controls how point properties and attributes are blended.

📋 _Visible when Blend Properties is enabled_

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Data Details</strong> <code>FPCGExPointsToBoundsDataDetails</code></summary>

Controls which computed values are written to data-domain attributes.

| Property                 | Type   | Default | Description                    |
| ------------------------ | ------ | ------- | ------------------------------ |
| **Write Transform**      | `bool` | `false` | Write computed transform       |
| **Write Density**        | `bool` | `true`  | Write blended density          |
| **Write Bounds Min**     | `bool` | `true`  | Write bounds minimum corner    |
| **Write Bounds Max**     | `bool` | `true`  | Write bounds maximum corner    |
| **Write Color**          | `bool` | `true`  | Write blended color            |
| **Write Steepness**      | `bool` | `true`  | Write blended steepness        |
| **Write Best Fit Plane** | `bool` | `true`  | Write best-fit plane transform |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Points Count</strong> <code>bool</code></summary>

When enabled, writes the number of merged points to an attribute.

Default: `false`

</details>

<details>

<summary><strong>Points Count Attribute Name</strong> <code>FName</code></summary>

Attribute name for the merged points count.

Default: `@Data.MergedPointsCount`

📋 _Visible when Write Points Count is enabled_

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                                                         |
| ------- | ---------- | ----------------------------------------------------------------------------------- |
| **Out** | Point Data | Single bounds point (Collapse) or original points with data attributes (Write Data) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/PCGExPointsToBounds.h)
