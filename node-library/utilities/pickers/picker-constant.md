---
description: A Picker that has a single value.
icon: circle-dashed
---

# Picker : Constant

### Overview

This picker selects a single constant index from a collection. It's the simplest picker type, using a fixed value that you specify directly in the node settings. The value can be either a discrete index (like "select item 5") or a normalized position (like "select the item at 50% through the collection").

### How It Works

1. **Mode Selection**: Depending on the Treat As Normalized setting:
   * **Discrete Mode**: Uses the DiscreteIndex value directly as an index
   * **Normalized Mode**: Uses the RelativeIndex value as a 0-1 position and converts it to an index
2. **Negative Index Support**: Negative values select from the end of the collection
3. **Index Resolution**: Converts the configured value into a single index
4. **Safety Application**: Applies the configured Safety mode to handle out-of-bounds indices
5. **Output**: Returns a single-element set containing the selected index

### Behavior

**Collection of 10 points (indices 0-9):**

```
Discrete Mode:
DiscreteIndex: 0  → Selects index 0 (first item)
DiscreteIndex: 5  → Selects index 5 (sixth item)
DiscreteIndex: 9  → Selects index 9 (last item)
DiscreteIndex: -1 → Selects index 9 (last item)
DiscreteIndex: -3 → Selects index 7 (third from end)
```

**Normalized Mode (0.0 to 1.0):**

```
RelativeIndex: 0.0  → Selects index 0 (0% = first)
RelativeIndex: 0.5  → Selects index 5 (50% = middle)
RelativeIndex: 1.0  → Selects index 9 (100% = last)
RelativeIndex: 0.25 → Selects index 2 or 3 (depends on TruncateMode)
```

**Out-of-Bounds with Safety:**

```
DiscreteIndex: 15 (collection only has 10)
- Safety = Ignore: No selection
- Safety = Clamp: Selects index 9 (clamped to max)
- Safety = Wrap: Selects index 5 (15 % 10 = 5)
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Discrete Index</strong> <code>int32</code></summary>

The fixed index to select when using discrete mode. This is a direct index into the collection.

Use negative values to select from the end:

* **-1**: Last item
* **-2**: Second-to-last item
* **-3**: Third-to-last item, etc.

Default: `0`

📋 _Visible when Treat As Normalized is disabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Relative Index</strong> <code>double</code></summary>

The normalized position (0.0 to 1.0) to select when using normalized mode. This value is converted to a discrete index based on the collection size.

* **0.0**: First item (0%)
* **0.5**: Middle item (50%)
* **1.0**: Last item (100%)

The TruncateMode setting determines how fractional positions are converted to indices.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExPickers-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExPickers/Public/Pickers/PCGExPickerConstant.h)
