---
description: Creates a bitmask configuration for efficient multi-flag storage.
icon: circle
---

# Bitmask

### Overview

This node creates a bitmask configuration that can be used to store and manipulate multiple boolean flags efficiently in a single integer value. Bitmasks use individual bits as flags, allowing up to 64 boolean values to be stored in a single 64-bit integer. The node supports individual bit manipulation, bit mutations through operations, and composition from multiple bitmask references, making it a powerful tool for state management and flag-based logic.

### How It Works

1. **Mode Selection**: Chooses how the bitmask is defined:
   * **Individual**: Define each bit position directly
   * **Mutations**: Apply bitwise operations to modify the mask
   * **Compositions**: Combine multiple bitmask references
2. **Bitmask Creation**: Constructs the integer value representing the flag states
3. **Mutations**: Optionally applies bitwise operations (AND, OR, XOR, NOT) to modify bits
4. **Compositions**: Optionally combines with other bitmask references
5. **Output**: Produces a bitmask configuration for use in other nodes

### Behavior

```
Bitmask Basics:

Bit positions (0-63) represent individual flags
Bit Value:   0 = Flag OFF
             1 = Flag ON
```

```
Example 8-bit bitmask:

Binary:  01010011
Decimal: 83
Flags:   [0]=ON, [1]=ON, [4]=ON, [6]=ON (others OFF)
```

```
Individual Mode:

Directly set the integer value
Bitmask = 5 = binary 0101 → Flags 0 and 2 ON
```

```
Mutations:

Start with base bitmask, apply operations:
Base:      0101
OR 0010:   0111 (added flag 1)
AND 1101:  0101 (cleared flag 1)
XOR 0011:  0110 (toggled flags 0,1)
```

```
Compositions:

Combine multiple bitmask references:
MaskA = 0011
MaskB = 0101
Combined (OR) = 0111
Combined (AND) = 0001
```

```
Use Cases:

- Feature flags (enabled/disabled states)
- State machines (multiple concurrent states)
- Permission systems (capability flags)
- Classification (multiple categories)
- Visibility masks (show/hide layers)
```

Good for: state management, feature flags, multi-category classification, permission systems, visibility control

### Settings

<details>

<summary><strong>Bitmask</strong> <code>FPCGExBitmask</code></summary>

The bitmask configuration defining the flag values and operations.

**Mode**: How the bitmask is defined

* **Individual**: Direct integer value
* **Mutations**: Apply bitwise operations
* **Compositions**: Combine bitmask references

**Bitmask**: The base integer value (0-9223372036854775807 for 64-bit)

* Each bit position represents a flag
* Bit 0 = value 1, Bit 1 = value 2, Bit 2 = value 4, etc.

**Mutations**: Array of bit operations to apply

* AND: Keep only bits present in both
* OR: Combine bits from both
* XOR: Toggle bits that differ
* NOT: Invert all bits

**Compositions**: Array of bitmask references to combine

* Allows building complex masks from simpler ones
* Useful for hierarchical or modular flag systems

⚡ PCG Overridable (Bitmask value)

</details>

<details>

<summary><strong>Title Char Limit</strong> <code>int32</code></summary>

Maximum number of characters to display in the node title when showing the bitmask value.

Limits the display length for readability in the graph editor.

Default: `32`

</details>

### Bitmask Operations

**Common Bitwise Operations:**

| Operation | Symbol | Description               | Example             |
| --------- | ------ | ------------------------- | ------------------- |
| AND       | &      | Keep bits present in both | 0101 & 0011 = 0001  |
| OR        | \|     | Combine bits from both    | 0101 \| 0011 = 0111 |
| XOR       | ^      | Toggle differing bits     | 0101 ^ 0011 = 0110  |
| NOT       | \~     | Invert all bits           | \~0101 = 1010       |

**Flag Checking:**

* Test flag N: `(mask & (1 << N)) != 0`
* Set flag N: `mask |= (1 << N)`
* Clear flag N: `mask &= ~(1 << N)`
* Toggle flag N: `mask ^= (1 << N)`

### Outputs

| Pin         | Type    | Description                                                                     |
| ----------- | ------- | ------------------------------------------------------------------------------- |
| **Bitmask** | Bitmask | Bitmask configuration for use in filtering, classification, or state management |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Bitmasks/PCGExBitmask.h)
