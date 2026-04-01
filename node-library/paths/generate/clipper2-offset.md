---
description: Does a Clipper2 offset operation with optional dual (inward+outward) offset.
icon: circle
---

# Clipper2 : Offset

### Overview

This node expands or contracts paths by a specified distance using the Clipper2 library. Positive offset values expand paths outward, while negative values contract them inward. The node supports multiple iterations, allowing creation of concentric offset rings, and configurable corner and endpoint styles for different visual results.

> This is a [clipper2-processor.md](../common-settings/clipper2-processor.md "mention")

### How It Works

1. **Project to 2D**: Input paths are projected onto a 2D plane using the specified projection settings.
2. **Apply Offset**: Each path is expanded or contracted by the offset amount using Clipper2's offset algorithm.
3. **Iterate**: If multiple iterations are specified, the offset is applied repeatedly, creating concentric rings.
4. **Style Corners/Ends**: Corner joints and path endpoints are shaped according to the selected join and end type settings.
5. **Unproject Results**: Resulting paths are transformed back to 3D space with interpolated attributes.

**Usage Notes**

* **Negative Offset**: Negative offset values contract paths inward. Very large negative values may cause paths to disappear or invert.
* **Multiple Iterations**: With iterations > 1, multiple offset rings are generated. The offset is applied cumulatively, so iteration 2 is offset from iteration 1, not from the original.
* **Dual Offset**: The "Dual" tag identifies paths from negative (inward) offset when both directions are processed.
* **Open Paths**: End type settings control how open path endpoints are handled. "Polygon" closes them, while other options create various cap styles.

### Behavior

```
Offset Operation:

Original Path    Positive Offset    Negative Offset
    ┌───┐           ┌─────┐            ┌─┐
    │   │     →     │     │      or    │ │
    └───┘           │     │            └─┘
                    └─────┘

Multiple Iterations (offset = 10, iterations = 3):
    ┌───┐     ←── Original
  ┌─┤   ├─┐   ←── Iteration 1 (+10)
┌─┤ │   │ ├─┐ ←── Iteration 2 (+20)
│ │ │   │ │ │ ←── Iteration 3 (+30)
```

### Inputs

| Pin    | Type  | Description           |
| ------ | ----- | --------------------- |
| **In** | Paths | Input paths to offset |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Controls how 3D paths are projected to 2D for Clipper2 processing.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

#### Iteration Settings

<details>

<summary><strong>Iterations</strong> <code>FPCGExInputShorthandNameInteger32Abs</code></summary>

Number of offset iterations to apply. Each iteration offsets from the previous result, creating concentric rings.

| Property      | Type  | Description                               |
| ------------- | ----- | ----------------------------------------- |
| **Input**     | enum  | Constant or Attribute                     |
| **Attribute** | FName | Attribute name when using attribute input |
| **Constant**  | int32 | Fixed iteration count when using constant |

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>├─ Consolidation</strong> <code>EPCGExClipper2OffsetIterationCount</code></summary>

When processing groups with different iteration values per source path, this determines how to resolve the final iteration count.

| Option      | Description                          |
| ----------- | ------------------------------------ |
| **First**   | Use the first path's iteration value |
| **Last**    | Use the last path's iteration value  |
| **Average** | Average all iteration values         |
| **Min**     | Use the minimum iteration value      |
| **Max**     | Use the maximum iteration value      |

Default: `Max`

⚡ PCG Overridable

</details>

<details>

<summary><strong>└─ Min Iterations</strong> <code>int32</code></summary>

Minimum guaranteed number of iterations, regardless of attribute values.

Default: `1`

Minimum: 0

⚡ PCG Overridable

</details>

#### Offset Settings

<details>

<summary><strong>Offset</strong> <code>FPCGExInputShorthandSelectorDouble</code></summary>

The offset distance to apply. Positive values expand outward, negative values contract inward.

| Property      | Type     | Description                   |
| ------------- | -------- | ----------------------------- |
| **Input**     | enum     | Constant or Attribute         |
| **Attribute** | selector | Attribute to read offset from |
| **Constant**  | double   | Fixed offset value            |

Default: `10`

⚡ PCG Overridable

</details>

<details>

<summary><strong>└─ Scale</strong> <code>double</code></summary>

Multiplier applied to the offset value. Useful when using attributes to scale the effect globally.

Default: `1.0`

⚡ PCG Overridable

</details>

#### Corner & Endpoint Settings

<details>

<summary><strong>Join Type</strong> <code>EPCGExClipper2JoinType</code></summary>

Controls the shape of corners where path segments meet.

| Option     | Description                                                      |
| ---------- | ---------------------------------------------------------------- |
| **Square** | Right-angle corners with sharp 90° edges                         |
| **Round**  | Smooth curved corners following an arc                           |
| **Bevel**  | Chamfered corners with a flat cut across                         |
| **Miter**  | Extended pointed corners (use Miter Limit to cap extreme angles) |

Default: `Round`

⚡ PCG Overridable

</details>

<details>

<summary><strong>└─ Miter Limit</strong> <code>double</code></summary>

Limits how far miter corners can extend. At sharp angles, miter joins can become very long; this caps them by switching to bevel when exceeded.

Default: `2.0`

Minimum: 1.0

⚡ PCG Overridable

</details>

<details>

<summary><strong>End Type (Closed)</strong> <code>EPCGExClipper2EndType</code></summary>

How to handle endpoints for closed paths.

| Option      | Description                                         |
| ----------- | --------------------------------------------------- |
| **Polygon** | Treat as closed polygon (standard for closed loops) |
| **Joined**  | Create thin double-sided paths                      |
| **Butt**    | Flat perpendicular end caps                         |
| **Square**  | Extended square end caps                            |
| **Round**   | Smooth semicircular end caps                        |

Default: `Polygon`

⚡ PCG Overridable

</details>

<details>

<summary><strong>End Type (Open)</strong> <code>EPCGExClipper2EndType</code></summary>

How to handle endpoints for open paths.

| Option      | Description                         |
| ----------- | ----------------------------------- |
| **Polygon** | Close the path and treat as polygon |
| **Joined**  | Create thin double-sided paths      |
| **Butt**    | Flat perpendicular end caps         |
| **Square**  | Extended square end caps            |
| **Round**   | Smooth semicircular end caps        |

Default: `Round`

📋 _Visible when Skip Open Paths is disabled_

⚡ PCG Overridable

</details>

#### Output Settings

<details>

<summary><strong>Write Iteration</strong> <code>bool</code></summary>

When enabled, writes the iteration index (0-based) to an attribute on each output path.

Default: `false`

</details>

<details>

<summary><strong>Iteration Attribute Name</strong> <code>FString</code></summary>

The attribute name to write the iteration index to.

Default: `Iteration`

📋 _Visible when Write Iteration is enabled_

⚡ PCG Overridable

</details>

#### Tagging Settings

<details>

<summary><strong>Tag Iteration</strong> <code>bool</code></summary>

When enabled, adds a tag with the iteration index to each output path.

Default: `false`

</details>

<details>

<summary><strong>Iteration Tag</strong> <code>FString</code></summary>

The tag prefix for iteration tagging. Format: `{tag}:{index}`.

Default: `OffsetNum`

📋 _Visible when Tag Iteration is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Dual</strong> <code>bool</code></summary>

When enabled, adds a tag to paths generated from negative (inward) offset operations.

Default: `false`

</details>

<details>

<summary><strong>Dual Tag</strong> <code>FString</code></summary>

The tag applied to dual (negative offset) paths.

Default: `Dual`

📋 _Visible when Tag Dual is enabled_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common Clipper2 processing settings from its base class.

→ See Clipper2 Processor Settings for: Precision, Simplify Paths, Preserve Collinear, Arc Tolerance, Carry Over Details, Blending Details, Open Paths Output, Data Matching, and other shared settings.

### Outputs

| Pin            | Type  | Description                                   |
| -------------- | ----- | --------------------------------------------- |
| **Out**        | Paths | Offset paths (all iterations combined)        |
| **Open Paths** | Paths | Open paths (when Output Pin mode is selected) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClipper2-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClipper2/Public/Elements/PCGExClipper2Offset.h)
