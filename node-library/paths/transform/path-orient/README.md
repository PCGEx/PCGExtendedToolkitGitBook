---
description: Orient path points.
icon: circle
---

# Path : Orient

### Overview

This node rotates path points so they face along the path direction. The orientation method determines how the forward direction is computed from neighboring points, and the result can be applied directly to point transforms or output to an attribute.

### How It Works

1. **Build Path**: Analyze the path to compute directions between neighboring points.
2. **Compute Orientation**: For each point, use the selected orientation method to determine the forward direction.
3. **Build Transform**: Construct a rotation aligning the orient axis with the computed direction and the up axis with the configured up vector.
4. **Apply or Output**: Either apply the rotation to the point's transform or write it to an attribute.

**Usage Notes**

* **Orientation Methods**: Different methods produce different results at corners - Average smooths transitions, LookAt points directly at targets, Weighted biases toward closer neighbors.
* **Flip Conditions**: Use filters to flip orientation direction on specific points (useful for bidirectional paths or special cases).
* **Dot Product Output**: The dot product between previous/next directions indicates how sharp corners are (-1 = sharp turn, 1 = straight).

### Inputs

| Pin                    | Type             | Description                             |
| ---------------------- | ---------------- | --------------------------------------- |
| **In**                 | Points           | Path points to orient                   |
| **Overrides : Orient** | Orient Factories | Override orientation settings per-point |
| **Flip Conditions**    | Filter Factories | Filters to flip orientation direction   |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Orient Axis</strong> <code>EPCGExAxis</code></summary>

Which local axis of the point should align with the computed path direction.

| Option       | Description                      |
| ------------ | -------------------------------- |
| **Forward**  | +X axis (Unreal default forward) |
| **Backward** | -X axis                          |
| **Right**    | +Y axis                          |
| **Left**     | -Y axis                          |
| **Up**       | +Z axis                          |
| **Down**     | -Z axis                          |

Default: `Forward`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Axis</strong> <code>EPCGExAxis</code></summary>

Which local axis of the point should align with the up direction.

Default: `Up`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Orientation</strong> <code>UPCGExOrientInstancedFactory</code></summary>

The method used to compute orientation from path geometry.

| Option       | Description                                                                            |
| ------------ | -------------------------------------------------------------------------------------- |
| **Average**  | Averages directions to neighboring points for smooth transitions.                      |
| **Look At**  | Points toward a target (next point, previous point, attribute direction, or position). |
| **Weighted** | Distance-weighted blend of neighbor directions.                                        |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Flip Direction</strong> <code>bool</code></summary>

Default flip state for orientation. When true, the orientation is reversed (points backward along the path).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output</strong> <code>EPCGExOrientUsage</code></summary>

How to use the computed orientation.

| Option                  | Description                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| **Apply to Point**      | Directly modify the point's transform rotation.                             |
| **Output to Attribute** | Write the orientation as a transform attribute without modifying the point. |

Default: `ApplyToPoint`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Attribute</strong> <code>FName</code></summary>

Name of the transform attribute to write orientation to.

Default: `Orient`

📋 _Visible when Output = Output to Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Dot</strong> <code>bool</code></summary>

Write the dot product between directions to previous and next points. This measures how sharp the corner is at each point.

* `1.0` = Straight (no turn)
* `0.0` = Right angle turn
* `-1.0` = Complete reversal

Default: `false`

Attribute: `Dot`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExOrient.h)
