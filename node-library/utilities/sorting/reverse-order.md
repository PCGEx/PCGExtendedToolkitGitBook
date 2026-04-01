---
description: Simply reverse the order of points or change winding of paths.
icon: circle
---

# Reverse Order

### Overview

This node reverses the point order within each data collection, effectively flipping paths to traverse in the opposite direction. Reversing can be unconditional, based on sorting rules, or to achieve a specific winding direction (clockwise or counter-clockwise).

### How It Works

1. **Evaluate Condition**: Based on the method, determine if reversal is needed.
2. **Check Winding**: For winding mode, calculate current winding and compare to target.
3. **Reverse Points**: Flip the point order if the condition is met.
4. **Swap Attributes**: Optionally swap attribute pairs (e.g., arrive/leave tangents).
5. **Apply Tags**: Tag data based on whether it was reversed.

**Usage Notes**

* **Tangent Swapping**: When reversing paths with tangent attributes, use Swap Attributes to exchange ArriveTangent and LeaveTangent so curves remain correct.
* **Multiply by -1**: Enable this option when swapping directional attributes (like tangents) that need to point the opposite way after reversal.
* **Winding Mode**: Projects points to 2D to calculate winding. Use the projection settings to control which plane is used.
* **Consistent Winding**: Use winding mode to ensure all paths wind in the same direction (useful for extrusion, fill operations, etc.).

### Behavior

```
Reverse point order:

Before:     [1]───[2]───[3]───[4]───[5]
             →     →     →     →

After:      [5]───[4]───[3]───[2]───[1]
             →     →     →     →
            (now traverses in opposite direction)

Winding enforcement (target = Counter-Clockwise):

Clockwise path:       Counter-Clockwise (reversed):
    1───2                 4───3
    |   |        →        |   |
    4───3                 1───2
```

### Inputs

| Pin               | Type                 | Description                       |
| ----------------- | -------------------- | --------------------------------- |
| **In**            | Points               | Points to potentially reverse     |
| **Sorting Rules** | Sort Rules Factories | Rules to determine sort direction |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Method</strong> <code>EPCGExPointReverseMethod</code></summary>

How to determine when to reverse point order.

| Option            | Description                                           |
| ----------------- | ----------------------------------------------------- |
| **Unconditional** | Always reverse all point collections.                 |
| **Sorting Rules** | Use sorting rules to determine if reversal is needed. |
| **Winding**       | Reverse to achieve the target winding direction.      |

Default: `Unconditional`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Target sort direction when using sorting rules.

| Option         | Description              |
| -------------- | ------------------------ |
| **Ascending**  | Ensure ascending order.  |
| **Descending** | Ensure descending order. |

Default: `Ascending`

📋 _Visible when Method = Sorting Rules_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Winding</strong> <code>EPCGExWinding</code></summary>

Target winding direction. Paths with opposite winding will be reversed.

| Option                | Description                       |
| --------------------- | --------------------------------- |
| **Clockwise**         | Ensure clockwise winding.         |
| **Counter Clockwise** | Ensure counter-clockwise winding. |

Default: `Counter Clockwise`

📋 _Visible when Method = Winding_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Settings for projecting 3D points to 2D for winding calculation.

📋 _Visible when Method = Winding_

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

***

#### Attribute Swapping

<details>

<summary><strong>Swap Attributes Values</strong> <code>TArray</code></summary>

Pairs of attributes to swap values between when reversing. Useful for directional attributes like tangents.

| Property                  | Description                           |
| ------------------------- | ------------------------------------- |
| **First Attribute Name**  | First attribute in the swap pair.     |
| **Second Attribute Name** | Second attribute in the swap pair.    |
| **Multiply By Minus One** | Negate numeric values after swapping. |

⚡ PCG Overridable

</details>

***

#### Tagging

<details>

<summary><strong>Tag If Reversed</strong> <code>bool</code></summary>

Add a tag to data collections that were reversed.

Default: `true`

</details>

<details>

<summary><strong>Is Reversed Tag</strong> <code>FString</code></summary>

Tag to apply when points were reversed.

Default: `Reversed`

📋 _Visible when Tag If Reversed = true_

</details>

<details>

<summary><strong>Tag If Not Reversed</strong> <code>bool</code></summary>

Add a tag to data collections that were not reversed.

Default: `false`

</details>

<details>

<summary><strong>Is Not Reversed Tag</strong> <code>FString</code></summary>

Tag to apply when points were not reversed.

Default: `NotReversed`

📋 _Visible when Tag If Not Reversed = true_

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExReversePointOrder.h)
