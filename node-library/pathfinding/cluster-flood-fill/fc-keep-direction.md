---
description: Stop diffusion when the path direction changes beyond a threshold.
icon: circle-dashed
---

# FC : Keep Direction

### Overview

This fill control sub-node constrains diffusion to maintain a consistent direction. It compares the direction of each new edge against recent edges in the path, and stops expansion if the direction changes too much. This creates paths that tend to continue in straight or gently curving lines rather than zigzagging.

### How It Works

1. **Track Direction**: Maintains a window of recent edge directions along the current path.
2. **Compare Directions**: Computes the direction to the candidate and compares it against the windowed average.
3. **Hash Comparison**: Uses direction hashing to detect significant changes with configurable tolerance.
4. **Stop on Change**: If the new direction differs significantly from recent history, the candidate is rejected.

**Usage Notes**

* **Window Size**: Larger windows smooth out direction changes over more edges, allowing gradual turns. Smaller windows enforce stricter straight-line behavior.
* **Direction Hashing**: The hash comparison uses a tolerance to determine when directions are considered different.
* **Straight Paths**: Useful for creating paths that follow logical routes without backtracking or sharp turns.

### Behavior

```
Keep Direction:

Allowed path (gradual curve):
  Seed → ─ → ╲ → ╲ → ─ → Continue
        →   ↘   ↘   →
  (each step similar to previous direction)

Blocked path (sharp turn):
  Seed → ─ → ─ → ↓ STOP
        →   →   ↓
  (direction changed too much)

Window Effect:
  Window=1: Compare to immediate previous edge only
  Window=3: Compare to average of last 3 edges
```

### Settings

<details>

<summary><strong>Window Size Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the window size comes from a constant or attribute.

| Option        | Description                        |
| ------------- | ---------------------------------- |
| **Constant**  | Use a fixed window size            |
| **Attribute** | Read window size from an attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Window Size (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read the window size from.

📋 _Visible when Window Size Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Window Size</strong> <code>int32</code></summary>

Number of recent edges to consider when computing average direction. Larger values allow more gradual direction changes.

Default: `1`

Minimum: 1

📋 _Visible when Window Size Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Hash Comparison Details</strong> <code>FPCGExVectorHashComparisonDetails</code></summary>

Controls how direction vectors are compared to detect significant changes.

| Property      | Type   | Description                                               |
| ------------- | ------ | --------------------------------------------------------- |
| **Tolerance** | double | Angular tolerance for direction comparison (default: 0.1) |

Higher tolerance allows more direction variation before blocking.

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Fill Controls Base for: Source

Note: The Steps setting is not available for this control.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsFloodFill-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsFloodFill/Public/FillControls/PCGExFillControlKeepDirection.h)
