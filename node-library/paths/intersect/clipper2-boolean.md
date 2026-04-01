---
description: Does a Clipper2 Boolean operation.
icon: circle
---

# Clipper2 : Boolean

### Overview

This node performs 2D boolean operations on paths using the Clipper2 library. It supports union (combining shapes), intersection (finding overlapping areas), difference (subtracting one shape from another), and XOR (keeping non-overlapping areas). Paths are projected to 2D, processed, then unprojected back to 3D space.

> This is a [clipper2-processor.md](../common-settings/clipper2-processor.md "mention")

### How It Works

1. **Project to 2D**: Input paths are projected onto a 2D plane using the specified projection settings.
2. **Apply Boolean Operation**: The selected boolean operation is performed on the projected paths using Clipper2's integer-based algorithm.
3. **Handle Fill Rule**: The fill rule determines how overlapping and self-intersecting regions are treated during the operation.
4. **Unproject Results**: Resulting paths are transformed back to 3D space, blending attributes from source points.

**Usage Notes**

* **Union without Operands**: Union mode combines all input paths into a single merged shape. No separate operand input is needed.
* **Operand Pin**: For Intersection, Difference, and XOR operations, enable "Use Operand Pin" to provide a second set of paths as operands. Otherwise, all inputs are treated as a single group.
* **Winding Direction**: Path winding direction (clockwise vs counter-clockwise) affects how regions are identified as solid or hole. The fill rule interprets winding differently.

### Behavior

```
Boolean Operations:

Union:          Intersection:    Difference:      XOR:
  ┌───┐            ┌───┐          ┌───┐           ┌───┐
  │ A ├──┐         │ A ├──┐       │ A ├──┐        │ A ├──┐
  └─┬─┘  │         └─┬─┘  │       └─┬─┘  │        └─┬─┘  │
    │ ┌──┴─┐         │ ┌──┴─┐       │ ┌──┴─┐        │ ┌──┴─┐
    │ │ B  │         │ │ B  │       │ │ B  │        │ │ B  │
    └─┴────┘         └─┴────┘       └─┴────┘        └─┴────┘
       ↓                ↓              ↓               ↓
  ┌────────┐         ┌───┐         ┌───┐          ┌───┐
  │ A ∪ B  │         │A∩B│         │A-B│          │   ├──┐
  │        │         └───┘         └───┘          └─┬─┘  │
  └────────┘                                        └────┘
                                                    (A⊕B)
```

### Inputs

| Pin          | Type  | Description                                              |
| ------------ | ----- | -------------------------------------------------------- |
| **In**       | Paths | Subject paths for the boolean operation                  |
| **Operands** | Paths | Second set of paths for binary operations (when enabled) |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Controls how 3D paths are projected to 2D for Clipper2 processing.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

<details>

<summary><strong>Operation</strong> <code>EPCGExClipper2BooleanOp</code></summary>

The boolean operation to perform.

| Option           | Description                                                                        |
| ---------------- | ---------------------------------------------------------------------------------- |
| **Union**        | Combines all paths into a single merged shape. Overlapping regions become one.     |
| **Intersection** | Keeps only the areas where paths overlap. Empty result if no overlap.              |
| **Difference**   | Subtracts operand paths from subject paths. Cuts holes where they overlap.         |
| **XOR**          | Keeps areas that are in either shape but not in both. Toggles overlapping regions. |

Default: `Union`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fill Rule</strong> <code>EPCGExClipper2FillRule</code></summary>

Determines how overlapping and self-intersecting regions are treated.

| Option       | Description                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| **Even Odd** | Regions alternate between filled and empty based on how many boundaries are crossed. Good for complex self-intersecting shapes. |
| **Non Zero** | Regions are filled if their winding number is non-zero. Standard for most shapes.                                               |
| **Positive** | Only regions with positive (counter-clockwise) winding are filled.                                                              |
| **Negative** | Only regions with negative (clockwise) winding are filled.                                                                      |

Default: `NonZero`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Operand Pin</strong> <code>bool</code></summary>

When enabled, displays a separate "Operands" input pin for providing the second set of paths in binary operations (Intersection, Difference, XOR).

Default: `false`

📋 _Visible when Operation is not Union_

</details>

#### Inherited Settings

This node inherits common Clipper2 processing settings from its base class.

→ See Clipper2 Processor Settings for: Precision, Simplify Paths, Preserve Collinear, Arc Tolerance, Carry Over Details, Blending Details, Open Paths Output, Data Matching, and other shared settings.

### Outputs

| Pin            | Type  | Description                                   |
| -------------- | ----- | --------------------------------------------- |
| **Out**        | Paths | Resulting paths from the boolean operation    |
| **Open Paths** | Paths | Open paths (when Output Pin mode is selected) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClipper2-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClipper2/Public/Elements/PCGExClipper2Boolean.h)
