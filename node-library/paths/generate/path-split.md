---
description: Split existing paths into multiple new paths.
icon: circle
---

# Path : Split

### Overview

This node divides paths into multiple segments based on filter conditions. Points matching the filter trigger a split, with different actions determining exactly how the split occurs - duplicating split points, removing them, or using filter results to toggle between keeping and pruning sections.

### How It Works

1. **Evaluate Filters**: Test each point against the split condition filters.
2. **Identify Split Points**: Mark points where splitting should occur.
3. **Apply Split Action**: Based on the selected action, create new path segments.
4. **Handle Single Points**: Optionally omit paths that would only contain one point.
5. **Tag Results**: Apply even/odd tags to resulting path segments.

**Usage Notes**

* **Split vs Disconnect**: Split duplicates the split point so it appears in both paths. Disconnect keeps the split point in the current path and starts a new path from the next point.
* **Partition Mode**: Creates new paths only when the filter result changes, not at every matching point. Useful for grouping consecutive points with the same property.
* **Switch Mode**: Uses filter results as a toggle - consecutive true results are kept, consecutive false results are pruned. Creates paths from the "kept" sections.
* **Closed Loops**: Closed loops may result in paths that wrap around the original start/end point.

### Behavior

```
Split Action = Split:
                    ↓ split point
Before:     ●───●───●───●───●───●
After:      ●───●───●   ●───●───●
            (split point duplicated in both paths)

Split Action = Remove:
                    ↓ split point
Before:     ●───●───●───●───●───●
After:      ●───●       ●───●───●
            (split point removed)

Split Action = Disconnect:
                    ↓ split point
Before:     ●───●───●───●───●───●
After:      ●───●───●   ●───●───●
                    ↑   ↑
               (stays) (new path starts at next)

Switch Mode (toggle keep/prune):
Filter:     F   F   T   T   T   F   F   T
Result:             ●───●───●       ●
            (only "true" sections become paths)
```

### Inputs

| Pin                  | Type             | Description                        |
| -------------------- | ---------------- | ---------------------------------- |
| **In**               | Points           | Paths to split                     |
| **Split Conditions** | Filter Factories | Filters that identify split points |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Split Action</strong> <code>EPCGExPathSplitAction</code></summary>

How splitting is performed at matching points.

| Option         | Description                                                                         |
| -------------- | ----------------------------------------------------------------------------------- |
| **Split**      | Duplicate the split point - original becomes path end, copy becomes new path start. |
| **Remove**     | Remove the split point entirely, shrinking both adjacent paths.                     |
| **Disconnect** | Keep the split point in current path, start new path from the next point.           |
| **Partition**  | Like Split, but only creates new paths when filter result changes from previous.    |
| **Switch**     | Use filter result as a toggle between keeping and pruning points.                   |

Default: `Split`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Initial Behavior</strong> <code>EPCGExPathSplitInitialValue</code></summary>

For Switch/Partition modes, how the initial state is determined.

| Option                    | Description                                           |
| ------------------------- | ----------------------------------------------------- |
| **Constant**              | Start with a constant value (see bInitialValue).      |
| **Constant (Preserve)**   | Use constant but don't switch if first point matches. |
| **From Point**            | Use the first point's filter result as initial value. |
| **From Point (Preserve)** | Use first point's value but preserve its behavior.    |

Default: `Constant`

📋 _Visible when Split Action = Switch or Partition_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inclusive</strong> <code>bool</code></summary>

Whether the point causing a behavior change is included in the resulting segment. When true, the transition point is included; when false, it marks a boundary but isn't included.

Default: `false`

📋 _Visible when Split Action = Switch or Partition_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Single Point Outputs</strong> <code>bool</code></summary>

Don't output paths that contain only a single point. These can occur when split points are adjacent or at path endpoints.

Default: `true`

⚡ PCG Overridable

</details>

***

#### Tagging

<details>

<summary><strong>Tag If Even Split</strong> <code>bool</code></summary>

Apply a tag to even-numbered split results (0, 2, 4, ...).

Default: `true`

</details>

<details>

<summary><strong>Is Even Tag</strong> <code>FString</code></summary>

Tag to apply to even-numbered paths.

Default: `EvenSplit`

📋 _Visible when Tag If Even Split = true_

</details>

<details>

<summary><strong>Tag If Odd Split</strong> <code>bool</code></summary>

Apply a tag to odd-numbered split results (1, 3, 5, ...).

Default: `false`

</details>

<details>

<summary><strong>Is Odd Tag</strong> <code>FString</code></summary>

Tag to apply to odd-numbered paths.

Default: `OddSplit`

📋 _Visible when Tag If Odd Split = true_

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExSplitPath.h)
