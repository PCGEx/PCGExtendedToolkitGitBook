---
description: Combines multiple bitmasks using bitwise operations.
icon: circle
---

# Bitmask Merge

### Overview

This node merges multiple bitmask inputs into a single output bitmask by applying a specified bitwise operation. It allows combining flag sets from different sources, creating unions, intersections, or other logical combinations of multiple bitmask configurations. This is useful for aggregating state information, combining permission sets, or building complex multi-criteria filters from simpler components.

### How It Works

1. **Input Collection**: Receives multiple bitmask configurations from input pins
2. **Operation Selection**: Uses the specified bitwise operation (AND, OR, XOR, NOT, Set)
3. **Sequential Merging**: Applies the operation to combine bitmasks:
   * For two inputs: `Result = Bitmask1 OP Bitmask2`
   * For multiple inputs: `Result = (((Bitmask1 OP Bitmask2) OP Bitmask3) ...)`
4. **Output**: Produces a single merged bitmask configuration

### Behavior

```
OR Operation (Union - Combine Flags):

Input A: 0101 (flags 0,2)
Input B: 0011 (flags 0,1)
Output:  0111 (flags 0,1,2) ✓

Use case: Combine permissions from multiple sources
```

```
AND Operation (Intersection - Common Flags):

Input A: 0101 (flags 0,2)
Input B: 0011 (flags 0,1)
Output:  0001 (flag 0 only) ✓

Use case: Find shared features or common permissions
```

```
XOR Operation (Exclusive - Differing Flags):

Input A: 0101 (flags 0,2)
Input B: 0011 (flags 0,1)
Output:  0110 (flags 1,2 only) ✓

Use case: Highlight differences between flag sets
```

```
NOT Operation (Inversion):

Input A: 0101
Output:  1010 (all bits flipped) ✓

Use case: Invert enabled/disabled states
```

```
Set Operation (Replace):

Input A: 0101
Input B: 0011
Output:  0011 (uses last input) ✓

Use case: Override with latest bitmask
```

```
Multiple Inputs:

MaskA: 0001
MaskB: 0010
MaskC: 0100
OR result: 0111 (all flags combined)
AND result: 0000 (no common flags)
```

Good for: permission aggregation, state combination, multi-source filtering, flag composition, feature set merging

### Settings

<details>

<summary><strong>Operation</strong> <code>EPCGExBitOp</code></summary>

The bitwise operation to apply when merging input bitmasks.

| Operation        | Symbol | Description                                           | Use Case                                         |
| ---------------- | ------ | ----------------------------------------------------- | ------------------------------------------------ |
| **OR** (default) | \|     | Union - Combines all flags from all inputs            | Permission aggregation, feature enablement       |
| **AND**          | &      | Intersection - Only keeps flags present in ALL inputs | Finding common features, permission intersection |
| **XOR**          | ^      | Exclusive - Flags present in odd number of inputs     | Difference detection, toggle operations          |
| **NOT**          | \~     | Inversion - Flips all bits                            | State inversion, disable all enabled flags       |
| **Set**          | =      | Replace - Uses the last input value                   | Override, replacement                            |

**OR**: Most common for combining permissions or features **AND**: Useful for finding commonalities or enforcing all conditions **XOR**: Good for symmetric differences or toggle logic **NOT**: Inverts the entire bitmask **Set**: Simply uses the last input (useful for passthrough or override)

Default: `OR`

</details>

### Operation Details

**Order of Operations:**

When multiple bitmasks are provided, they are merged sequentially:

1. `Result = Bitmask[0]`
2. `Result = Result OP Bitmask[1]`
3. `Result = Result OP Bitmask[2]`
4. ... and so on

For OR, AND, and XOR, the order typically doesn't matter (commutative). For NOT, only the first input is inverted (subsequent inputs are ignored). For Set, only the last input matters.

**Practical Examples:**

```
Combining User Permissions:

UserPermissions: 0001 (Read)
GroupPermissions: 0110 (Write, Execute)
OR → 0111 (Read, Write, Execute)
```

```
Finding Required Features:

RequiredA: 1101
RequiredB: 1001
AND → 1001 (only features in both)
```

```
Toggling States:

CurrentState: 0101
ToggleMask: 0011
XOR → 0110 (toggle flags 0 and 1)
```

### Inputs

| Pin          | Type    | Description                              |
| ------------ | ------- | ---------------------------------------- |
| **Bitmasks** | Bitmask | Multiple bitmask configurations to merge |

### Outputs

| Pin         | Type    | Description           |
| ----------- | ------- | --------------------- |
| **Bitmask** | Bitmask | Merged bitmask result |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Bitmasks/PCGExBitmaskMerge.h)
