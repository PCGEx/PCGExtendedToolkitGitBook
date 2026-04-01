---
description: Filters points by indices using picker configurations.
icon: circle-plus
---

# Cherry Pick Points

### Overview

This node selects specific points from collections based on index values defined by picker factories. Points can be selected using constant indices, attribute-based indices, or ranges. The node supports inverted selection (discard picked instead of keep), separate output for discarded points, and can preserve or remove empty collections from the output.

### How It Works

1. **Picker Loading**: Loads picker factory configurations from the input pin
2. **Index Generation**: Each picker generates a set of point indices to select
3. **Index Collection**: Combines indices from all pickers into a selection set
4. **Point Filtering**: For each collection, keeps only points at selected indices
5. **Inversion** (optional): If enabled, keeps non-selected points instead
6. **Output Generation**: Creates filtered collections, optionally including discarded points

### Behavior

**Input Collection (10 points):**

```
Point indices: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9

Picker: Constant indices [1, 3, 5, 7]

Output (bInvert = false):
Kept: Points 1, 3, 5, 7 (4 points)
Discarded: Points 0, 2, 4, 6, 8, 9 (6 points)

Output (bInvert = true):
Kept: Points 0, 2, 4, 6, 8, 9 (6 points)
Discarded: Points 1, 3, 5, 7 (4 points)
```

**Multiple Pickers (Combined):**

```
Picker A: Indices [1, 3, 5]
Picker B: Indices [5, 7, 9]

Combined selection: [1, 3, 5, 7, 9] (union)

Output: Points at indices 1, 3, 5, 7, 9
```

**Output Options:**

```
bOutputDiscardedPoints = false:
- Out pin: Kept points only

bOutputDiscardedPoints = true:
- Out pin: Kept points
- Discarded pin: Discarded points (separate dataset)
```

**Empty Outputs:**

```
Collection A: 10 points, picker selects all → Output has 10 points
Collection B: 10 points, picker selects none → Output has 0 points

bAllowEmptyOutputs = false:
  Collection B is removed from output entirely

bAllowEmptyOutputs = true:
  Collection B output as empty collection (0 points)
```

**Range Picker Example:**

```
Picker: Range [2, 8] every 2
Indices selected: 2, 4, 6, 8

Input (12 points):
Output: Points 2, 4, 6, 8 (4 points)
```

**Attribute-Based Picker:**

```
Picker: Read from attribute "PickIndices"
Point 0: PickIndices = 3
Point 1: PickIndices = 7
Point 2: PickIndices = 1

Selected indices: [1, 3, 7]
Output: Points at indices 1, 3, 7
```

Good for: point selection, subset extraction, index-based filtering, manual picking, removing specific points

### Settings

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Whether to invert the picking logic.

* **false** (default): Keep picked indices, discard others
* **true**: Discard picked indices, keep others

Inverted picking is useful for exclusion operations: "remove these specific points."

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Discarded Points</strong> <code>bool</code></summary>

Whether to output discarded points to a separate dataset.

* **false** (default): Only output kept points
* **true**: Output both kept and discarded points on separate pins

Useful when you need both the selection and the remainder for different processing paths.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Empty Outputs</strong> <code>bool</code></summary>

Whether to allow point collections with zero points in the output.

* **false** (default): Remove collections that would be empty after filtering
* **true**: Keep empty collections in the output

When false, collections where all points are discarded are completely removed from the output stream.

Default: `false`

⚡ PCG Overridable

</details>

### Picker Factories

The node accepts picker factory inputs that define which point indices to select:

**Available Picker Types:**

* **Constant**: Fixed index or set of indices
* **Constant Range**: Index range with step (e.g., 0 to 100 every 10)
* **Attribute Set**: Read indices from point attributes
* **Attribute Set Ranges**: Read ranges from point attributes

Multiple pickers can be connected, and their selected indices are combined (union).

### Inputs

| Pin         | Type             | Description                    |
| ----------- | ---------------- | ------------------------------ |
| **In**      | Point Data       | Point collections to filter    |
| **Pickers** | Picker Factories | Index selection configurations |

### Outputs

| Pin                      | Type       | Description                                            |
| ------------------------ | ---------- | ------------------------------------------------------ |
| **Out**                  | Point Data | Filtered point collections (kept points)               |
| **Discarded** (optional) | Point Data | Discarded points (when Output Discarded Points = true) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExCherryPickPoints.h)
