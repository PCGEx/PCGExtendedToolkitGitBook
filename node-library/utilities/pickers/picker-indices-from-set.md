---
description: A Picker that accepts lists of values, read from one or more attributes.
icon: circle-dashed
---

# Picker : Indices from Set

### Overview

This picker reads index values from one or more attributes and uses them to select points from a collection. Each attribute can contain either discrete index values or normalized positions, and the picker aggregates all values from all specified attributes into a single set of picks. This is useful for selecting multiple specific points based on pre-computed or externally defined index lists.

### How It Works

1. **Attribute Reading**: Reads values from all specified attributes in the Attributes list
2. **Value Aggregation**: Collects all values from all attributes into a combined set
3. **Index Interpretation**: Depending on the Treat As Normalized setting:
   * **Discrete Mode**: Values are used directly as indices (5 = index 5)
   * **Normalized Mode**: Values are treated as 0-1 positions and converted to indices
4. **Negative Index Support**: Negative values select from the end (-1 = last item, -2 = second-to-last)
5. **Safety Application**: Applies the configured Safety mode to handle out-of-bounds indices
6. **Output**: Returns a set of unique indices ready for point selection

### Behavior

**Example with 2 attributes on a collection of 10 points:**

```
Attribute A values: [1, 3, 5]
Attribute B values: [7, 9]
```

**Discrete Mode:**

```
Combined picks: {1, 3, 5, 7, 9}
Selected points: indices 1, 3, 5, 7, 9
```

**Normalized Mode (values between 0-1):**

```
Attribute A: [0.1, 0.3, 0.5]  → [1, 3, 5] (10% = index 1, 30% = index 3, etc.)
Attribute B: [0.7, 0.9]       → [7, 9]
Selected points: indices 1, 3, 5, 7, 9
```

**Negative Indices (10 points):**

```
Values: [0, -1, -2, 5]
Resolved: [0, 9, 8, 5] (negative indices count from end)
```

### Inputs

| Pin    | Type       | Description                                             |
| ------ | ---------- | ------------------------------------------------------- |
| **In** | Point Data | Point data containing the attributes to read picks from |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Attributes</strong> <code>TArray&#x3C;FPCGAttributePropertyInputSelector></code></summary>

List of attributes to read index values from. Each attribute should contain numeric values representing either discrete indices or normalized positions (depending on the Treat As Normalized setting).

The picker will aggregate all values from all specified attributes. If no attributes are specified, the picker will attempt to use the first available numeric attribute.

**Note**: Use negative values in attributes to select from the end of the collection (-1 = last item, -2 = second-to-last, etc.).

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExPickers-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExPickers/Public/Pickers/PCGExPickerAttributeSet.h)
