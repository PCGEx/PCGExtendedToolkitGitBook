---
description: A Picker that selects a range of values.
icon: circle-dashed
---

# Picker : Range

### Overview

This picker selects a contiguous range of indices from a collection. It defines a start and end position, then generates picks for all indices within that range (inclusive). The range can be specified using either discrete indices or normalized positions, making it easy to select blocks of sequential items like "indices 5 through 10" or "the middle 50% of items."

### How It Works

1. **Range Definition**: Depending on the Treat As Normalized setting:
   * **Discrete Mode**: Uses DiscreteStartIndex and DiscreteEndIndex as direct indices
   * **Normalized Mode**: Uses RelativeStartIndex and RelativeEndIndex as 0-1 positions and converts them to indices
2. **Range Sanitization**: Ensures start ≤ end, swapping if necessary
3. **Negative Index Support**: Negative values select from the end of the collection
4. **Range Expansion**: Generates all indices from start to end (inclusive)
5. **Safety Application**: Applies the configured Safety mode to handle out-of-bounds indices
6. **Output**: Returns a set containing all indices within the specified range

### Behavior

Collection of 10 points (indices 0-9):

**Discrete Mode:**

```
Range [2, 5]:    {2, 3, 4, 5}
Range [0, 9]:    {0, 1, 2, 3, 4, 5, 6, 7, 8, 9} (entire collection)
Range [7, 7]:    {7} (single item)
Range [-3, -1]:  {7, 8, 9} (last 3 items)
Range [3, -2]:   {3, 4, 5, 6, 7, 8} (index 3 to second-to-last)
```

**Normalized Mode (0.0 to 1.0):**

```
Range [0.0, 0.5]:   {0, 1, 2, 3, 4, 5} (first half)
Range [0.5, 1.0]:   {5, 6, 7, 8, 9} (second half)
Range [0.25, 0.75]: {2, 3, 4, 5, 6, 7} (middle 50%)
Range [0.0, 1.0]:   {0, 1, 2, 3, 4, 5, 6, 7, 8, 9} (entire collection)
```

**Auto-Swap (if start > end):**

```
Range [8, 3] → Sanitized to [3, 8] → {3, 4, 5, 6, 7, 8}
```

**Out-of-Bounds with Safety:**

```
Range [5, 15] (collection only has 10):
- Safety = Ignore: Generates {5, 6, 7, 8, 9}, skips invalid indices
- Safety = Clamp: Generates {5, 6, 7, 8, 9} (end clamped to 9)
- Safety = Wrap: Generates {5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5} (wraps around)
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Discrete Start Index</strong> <code>int32</code></summary>

The starting index of the range when using discrete mode (inclusive).

Use negative values to count from the end:

* **0**: First item
* **5**: Sixth item
* **-1**: Last item
* **-5**: Fifth-to-last item

Default: `0`

📋 _Visible when Treat As Normalized is disabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Relative Start Index</strong> <code>double</code></summary>

The normalized starting position (0.0 to 1.0) of the range when using normalized mode (inclusive). This value is converted to a discrete index based on the collection size.

* **0.0**: First item (0%)
* **0.25**: Quarter through collection (25%)
* **0.5**: Middle item (50%)

Default: `0`

📋 _Visible when Treat As Normalized is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Discrete End Index</strong> <code>int32</code></summary>

The ending index of the range when using discrete mode (inclusive).

Use negative values to count from the end:

* **9**: Tenth item
* **-1**: Last item
* **-2**: Second-to-last item

If end is less than start, they will be automatically swapped.

Default: `0`

📋 _Visible when Treat As Normalized is disabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Relative End Index</strong> <code>double</code></summary>

The normalized ending position (0.0 to 1.0) of the range when using normalized mode (inclusive). This value is converted to a discrete index based on the collection size.

* **0.5**: Middle item (50%)
* **0.75**: Three-quarters through collection (75%)
* **1.0**: Last item (100%)

If end is less than start, they will be automatically swapped.

Default: `0`

📋 _Visible when Treat As Normalized is enabled_

⚡ PCG Overridable

</details>

#### Inherited Settings

This picker inherits common settings from the base picker configuration.

→ See Picker Factory Provider for: Treat As Normalized, Truncate Mode, Safety, and other shared picker properties.

### Outputs

| Pin        | Type   | Description                                                        |
| ---------- | ------ | ------------------------------------------------------------------ |
| **Picker** | Picker | Picker factory that can be used by nodes requiring index selection |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExPickers-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExPickers/Public/Pickers/PCGExPickerConstantRange.h)
