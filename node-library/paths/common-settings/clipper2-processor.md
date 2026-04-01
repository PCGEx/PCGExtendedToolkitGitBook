---
description: Base class providing common settings for all Clipper2-based path operations.
icon: code-commit
---

# Clipper2 Processor

### Overview

This abstract base class provides shared configuration for nodes that use the Clipper2 library to perform 2D boolean operations (union, intersection, difference, XOR) and path offset/inflate operations. All Clipper2 nodes inherit these settings for precision control, path handling, attribute blending, and output configuration.

Clipper2 operates on 2D integer coordinates internally, so paths are projected onto a 2D plane, processed, then unprojected back to 3D space.

### Common Settings

#### Processing Settings

<details>

<summary><strong>Main Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how input data is grouped for processing. When enabled, paths with matching attributes or tags are grouped together for combined operations.

| Property               | Type  | Description                                    |
| ---------------------- | ----- | ---------------------------------------------- |
| **Mode**               | enum  | Matching mode (Disabled, Attribute, Tag, etc.) |
| **Cluster Match Mode** | enum  | How cluster components match (Vtx, Edge)       |
| **Split Unmatched**    | bool  | Output unmatched data separately               |
| **Output Unmatched**   | bool  | Include unmatched data in output               |
| **Limit Matches**      | bool  | Cap number of matches per group                |
| **Limit**              | int32 | Maximum matches when limiting                  |

</details>

<details>

<summary><strong>Skip Open Paths</strong> <code>bool</code></summary>

When enabled, paths that are not closed loops are excluded from processing. Open paths cannot participate in boolean operations that require enclosed areas.

Default: `false`

⚡ PCG Overridable

</details>

#### Tweaks Settings

<details>

<summary><strong>Precision</strong> <code>int32</code></summary>

Decimal precision multiplier for Clipper2's integer coordinate system. Clipper2 uses 64-bit integers internally for extreme precision. This value scales floating-point coordinates up before processing and back down afterward. Higher values preserve more decimal precision but may affect performance.

Default: `100`

Minimum: 1

⚡ PCG Overridable

</details>

<details>

<summary><strong>Simplify Paths</strong> <code>bool</code></summary>

When enabled, performs path simplification on the output to remove redundant points.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve Collinear</strong> <code>bool</code></summary>

When enabled, keeps collinear points in the output. When disabled, consecutive points that lie on the same line are merged, reducing point count.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Arc Tolerance</strong> <code>double</code></summary>

Controls the accuracy of arc approximation when generating curved offsets (round joins/ends). Higher values produce fewer points but coarser curves. This value is scaled by the Precision setting internally.

Default: `5.0`

⚡ PCG Overridable

</details>

#### Output Settings

<details>

<summary><strong>Carry Over Details</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags from input paths are transferred to output paths.

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

Controls how point attributes are blended when paths are combined or new intersection points are created.

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Open Paths Output</strong> <code>EPCGExClipper2OpenPathOutput</code></summary>

Controls how open (non-closed) paths are handled in the output.

| Option           | Description                                     |
| ---------------- | ----------------------------------------------- |
| **Ignore**       | Discard open paths from output                  |
| **Output**       | Include open paths on the main output pin       |
| **Output (Pin)** | Route open paths to a separate "Open Paths" pin |

Default: `Output`

</details>

<details>

<summary><strong>Tag Holes</strong> <code>bool</code></summary>

When enabled, paths that represent holes (interior cutouts) are tagged with a specific tag.

Default: `false`

</details>

<details>

<summary><strong>Hole Tag</strong> <code>FString</code></summary>

The tag applied to paths identified as holes.

Default: `Hole`

📋 _Visible when Tag Holes is enabled_

</details>

#### Advanced Settings

<details>

<summary><strong>Union Group Before Operation</strong> <code>bool</code></summary>

Debug option. When enabled, performs a union of all paths in the subject group before the main operation.

Default: `false`

</details>

<details>

<summary><strong>Union Operands Before Operation</strong> <code>bool</code></summary>

Debug option. When enabled, performs a union of all paths in the operand group before the main operation.

Default: `false`

</details>

### Common Enums

#### EPCGExClipper2JoinType

Controls the shape of corners when offsetting paths.

| Option     | Description              |
| ---------- | ------------------------ |
| **Square** | Right-angle corners      |
| **Round**  | Smooth curved corners    |
| **Bevel**  | Chamfered/cut corners    |
| **Miter**  | Extended pointed corners |

#### EPCGExClipper2EndType

Controls how path endpoints are handled during offset operations.

| Option      | Description                                         |
| ----------- | --------------------------------------------------- |
| **Polygon** | Treat as closed polygon (ignores open/closed state) |
| **Joined**  | Join endpoints creating a thin double-sided path    |
| **Butt**    | Flat perpendicular end caps                         |
| **Square**  | Extended square end caps                            |
| **Round**   | Smooth semicircular end caps                        |

#### EPCGExClipper2FillRule

Determines how overlapping regions are treated during boolean operations.

| Option       | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| **Even Odd** | Alternating regions are filled/empty based on crossing count |
| **Non Zero** | Regions with non-zero winding number are filled              |
| **Positive** | Only regions with positive winding are filled                |
| **Negative** | Only regions with negative winding are filled                |

### Inputs

| Pin        | Type   | Description                                        |
| ---------- | ------ | -------------------------------------------------- |
| **In**     | Paths  | Main input paths (subjects for boolean operations) |
| **Labels** | Params | Optional labels for data matching                  |

### Outputs

| Pin            | Type  | Description                                   |
| -------------- | ----- | --------------------------------------------- |
| **Out**        | Paths | Processed output paths                        |
| **Open Paths** | Paths | Open paths (when Output Pin mode is selected) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClipper2-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClipper2/Public/Core/PCGExClipper2Processor.h)
