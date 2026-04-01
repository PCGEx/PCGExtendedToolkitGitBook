---
description: Applies bitwise operations to point attributes.
icon: circle
---

# Bitmask Operation

### Overview

This node performs bitwise operations on int64 point attributes, allowing manipulation of individual flag bits within each point's data. It can set flags, combine flags (OR), filter flags (AND), toggle flags (XOR), or invert flags (NOT) using either a constant bitmask value or per-point values from another attribute. This enables efficient multi-flag state management at the point level.

### How It Works

1. **Attribute Selection**: Identifies the target int64 attribute to modify (FlagAttribute)
2. **Mask Source**: Determines the bitmask value from:
   * **Constant**: Same mask applied to all points
   * **Attribute**: Per-point mask values from another attribute
3. **Operation Application**: For each point, applies the bitwise operation:
   * `Result = FlagAttribute OP Bitmask`
4. **Point Processing**: Processes points in parallel batches for efficiency
5. **Output**: Writes modified bitmask values back to the attribute

### Behavior

```
Set Operation (Replace):

Before: Point.Flags = 0101
Mask:   0011
After:  Point.Flags = 0011 (replaced)
```

```
OR Operation (Add Flags):

Before: Point.Flags = 0101
Mask:   0011 (flags to add)
After:  Point.Flags = 0111 (combined)
```

```
AND Operation (Filter Flags):

Before: Point.Flags = 0111
Mask:   0101 (flags to keep)
After:  Point.Flags = 0101 (filtered)
```

```
XOR Operation (Toggle Flags):

Before: Point.Flags = 0101
Mask:   0011 (flags to toggle)
After:  Point.Flags = 0110 (toggled)
```

```
NOT Operation (Invert All):

Before: Point.Flags = 0101
After:  Point.Flags = 1010 (inverted)
```

```
Constant vs Attribute Mask:

Constant: Same mask for all points
  Point[0].Flags OR 0011 = ...
  Point[1].Flags OR 0011 = ...

Attribute: Per-point masks
  Point[0].Flags OR Point[0].Mask = ...
  Point[1].Flags OR Point[1].Mask = ...
```

```
Common Use Cases:

- Enable feature: OR with flag bit
- Disable feature: AND with ~flag bit
- Toggle feature: XOR with flag bit
- Clear all: Set to 0
- Check flag: AND with flag bit, test != 0
```

Good for: point-level state management, feature flags, visibility control, classification flags, condition marking

### Settings

<details>

<summary><strong>Flag Attribute</strong> <code>FName</code></summary>

The point attribute to apply the bitmask operation to. Must be an int64 attribute.

Examples:

* `"Flags"`: Common flag storage attribute
* `"State"`: State machine flags
* `"Permissions"`: Access control flags
* `"Categories"`: Multi-category classification

The attribute must exist and be of type int64. If it doesn't exist, the node will fail.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operation</strong> <code>EPCGExBitOp</code></summary>

The bitwise operation to apply.

| Operation | Symbol | Description                     | Use Case                             |
| --------- | ------ | ------------------------------- | ------------------------------------ |
| **Set**   | =      | Replace flag value with mask    | Reset flags, set absolute state      |
| **OR**    | \|     | Add flags from mask             | Enable features, add classifications |
| **AND**   | &      | Keep only flags present in mask | Filter flags, enforce requirements   |
| **XOR**   | ^      | Toggle flags present in mask    | Switch states, invert specific flags |
| **NOT**   | \~     | Invert all flags                | Reverse state, complement flags      |

**Set**: Overwrites the attribute value **OR**: Adds flags without removing existing ones **AND**: Removes flags not in the mask **XOR**: Flips flags specified in the mask **NOT**: Ignores mask, inverts all bits

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mask Input</strong> <code>EPCGExInputValueType</code></summary>

Determines where the bitmask value comes from.

| Option                 | Description                               |
| ---------------------- | ----------------------------------------- |
| **Constant** (default) | Uses a fixed Bitmask value for all points |
| **Attribute**          | Reads mask from MaskAttribute per-point   |

**Constant**: Efficient, same operation on all points **Attribute**: Flexible, per-point custom masks

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mask Attribute</strong> <code>FName</code></summary>

The attribute to read the mask value from when MaskInput is Attribute. Must be int64.

Examples:

* `"CustomMask"`: Per-point operation masks
* `"AddFlags"`: Flags to add per point
* `"FilterMask"`: Per-point filter criteria

Only used when MaskInput = Attribute.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Bitmask (Constant)</strong> <code>int64</code></summary>

The constant bitmask value to apply when MaskInput is Constant.

Examples:

* `1` (binary 0001): Flag 0
* `3` (binary 0011): Flags 0 and 1
* `15` (binary 1111): Flags 0, 1, 2, 3
* `0`: Clear all flags (with Set operation)
* `-1`: All flags enabled (with Set operation)

Only used when MaskInput = Constant.

⚡ PCG Overridable

</details>

### Practical Examples

**Enable Feature Flags:**

```
Operation: OR
Bitmask: 0001 (enable flag 0)
Result: All points get flag 0 enabled
```

**Disable Specific Flags:**

```
Operation: AND
Bitmask: 1101 (keep all except flag 1)
Result: Flag 1 disabled on all points
```

**Toggle Debug Mode:**

```
Operation: XOR
Bitmask: 1000 (toggle flag 3)
Result: Points with flag 3 ON → OFF, OFF → ON
```

**Reset Flags:**

```
Operation: Set
Bitmask: 0 (clear all)
Result: All flags cleared
```

### Inputs

| Pin    | Type       | Description                       |
| ------ | ---------- | --------------------------------- |
| **In** | Point Data | Points with int64 flag attributes |

### Outputs

| Pin     | Type       | Description                          |
| ------- | ---------- | ------------------------------------ |
| **Out** | Point Data | Points with modified flag attributes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Bitmasks/PCGExBitwiseOperation.h)
