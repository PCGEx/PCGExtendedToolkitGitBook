---
description: Converts enum values into PCG constants.
icon: circle
---

# Enum Constant

### Overview

This node extracts values from C++ or Blueprint enums and outputs them as PCG data (attributes, strings, or tags). It can output a single enum value, all values, or a selection of values either to one pin or to multiple separate output pins. The node provides flexible attribute configuration, allowing output of enum keys (names), descriptions (display names), numeric values, and optional bitflag representations.

### How It Works

1. **Enum Selection**: Chooses the source enum via Picker or Selector
2. **Output Mode**: Determines what to output (Single, All, or Selection)
3. **Value Extraction**: Extracts enum metadata (keys, values, descriptions)
4. **Attribute Generation**: Creates point data with requested attribute combinations
5. **Pin Staging**: Routes data to single pin or multiple pins based on mode
6. **Optional Bitflags**: Converts enum selections to bitmask representation

### Behavior

#### Output Modes:

**Single:**

```
Outputs one enum value as a single point
```

**All:**

```
Enum with: Red=0, Green=1, Blue=2
Output: 3 points with attributes
  Point[0]: Value=0, Key="Red", Description="Red Color"
  Point[1]: Value=1, Key="Green", Description="Green"
  Point[2]: Value=2, Key="Blue", Description="Blue"
```

**All to Separate Outputs:**

```
Same 3 enum values → 3 separate output pins
  Pin "Red" → 1 point (Value=0)
  Pin "Green" → 1 point (Value=1)
  Pin "Blue" → 1 point (Value=2)
```

**Selection:**

```
Choose which values to output (e.g., Red and Blue)
Output: 2 points

Selection to Separate Outputs:
Selected values → separate pins
```

#### Attribute Options:

```
OutputEnumKeys = true → "Red", "Green", "Blue"
OutputEnumValues = true → 0, 1, 2
OutputEnumDescriptions = true → "Red Color", "Green", "Blue"

Namespace Stripping:
With: MyEnum::Red → "Red" (stripped)
Without: MyEnum::Red → "MyEnum::Red" (full)

Bitflags Output:
Selected: Red (0), Blue (2)
Output: Bitmask with bits 0 and 2 set
Binary: 0101 (flags at positions 0 and 2)
```

Good for: enum-driven workflows, dropdown selections, configuration parameters, state definitions, category lists

### Settings

<details>

<summary><strong>Source</strong> <code>EPCGExEnumConstantSourceType</code></summary>

How to select the enum to use.

| Option                 | Description                                   |
| ---------------------- | --------------------------------------------- |
| **Picker**             | Select enum from a dropdown picker            |
| **Selector** (default) | Use FEnumSelector for more flexible selection |

Default: `Selector`

</details>

<details>

<summary><strong>Output Mode</strong> <code>EPCGExEnumOutputMode</code></summary>

Determines what enum values to output and how.

| Mode                              | Description                                 |
| --------------------------------- | ------------------------------------------- |
| **Single**                        | Output one specific enum value              |
| **All** (default)                 | Output all enum values to a single pin      |
| **All to Separate Outputs**       | Output each enum value to its own pin       |
| **Selection**                     | Output selected enum values to a single pin |
| **Selection to Separate Outputs** | Output selected values to separate pins     |

Default: `All`

</details>

<details>

<summary><strong>Output Enum Keys</strong> <code>bool</code></summary>

Whether to output the enum key names (C++ identifier names).

* **false** (default): Don't output keys
* **true**: Output key attribute (e.g., "Red", "Green", "Blue")

Default: `false`

</details>

<details>

<summary><strong>Strip Enum Namespace From Key</strong> <code>bool</code></summary>

When outputting keys, strip the enum namespace prefix.

* **false**: Keep full name (e.g., "MyEnum::Red")
* **true** (default): Strip to short name (e.g., "Red")

Only used when OutputEnumKeys is true.

Default: `true`

</details>

<details>

<summary><strong>Key Attribute</strong> <code>FName</code></summary>

Attribute name for enum key output.

Default: `"Key"`

</details>

<details>

<summary><strong>Output Enum Descriptions</strong> <code>bool</code></summary>

Whether to output enum descriptions (human-readable display names).

* **false** (default): Don't output descriptions
* **true**: Output description attribute

Default: `false`

</details>

<details>

<summary><strong>Description Attribute</strong> <code>FName</code></summary>

Attribute name for enum description output.

Default: `"Description"`

</details>

<details>

<summary><strong>Output Enum Values</strong> <code>bool</code></summary>

Whether to output the numeric enum values.

* **false**: Don't output numeric values
* **true** (default): Output value attribute as int64

Default: `true`

</details>

<details>

<summary><strong>Value Output Attribute</strong> <code>FName</code></summary>

Attribute name for numeric value output.

Default: `"Value"`

</details>

<details>

<summary><strong>Output Flags</strong> <code>bool</code></summary>

Whether to output a bitmask representation of selected enum values.

* **false** (default): No bitflag output
* **true**: Generate bitmask with bits set for selected values

Useful for compact multi-selection representation.

Default: `false`

</details>

<details>

<summary><strong>Flags Name</strong> <code>FName</code></summary>

Attribute name for the bitflag output.

Only used when bOutputFlags is true.

Default: `"Flags"`

</details>

<details>

<summary><strong>Flag Bit Offset</strong> <code>uint8</code></summary>

Bit position to start writing enum bits. Allows offsetting the bitmask to avoid conflicts with other flag systems.

Examples:

* **0** (default): Start at bit 0
* **8**: Start at bit 8 (second byte)
* **16**: Start at bit 16 (third byte)

Only used when bOutputFlags is true.

Default: `0`

Range: `0` to `63`

</details>

### Use Cases

**Configuration Systems:**

* Output all configuration options as point data
* Use Selection mode for user-chosen options
* Generate bitflags for compact option storage

**Dropdown Implementations:**

* Convert enum to point data for dropdown population
* Output keys/descriptions for UI display
* Use numeric values for internal logic

**State Machines:**

* Output all possible states
* Use separate pins for routing by state
* Generate bitflags for multi-state tracking

**Category Systems:**

* All enum values as category list
* Selection mode for active categories
* Bitflags for efficient multi-category storage

### Outputs

| Pin                  | Type       | Description                                           |
| -------------------- | ---------- | ----------------------------------------------------- |
| **Out** (varies)     | Param Data | Enum value constants (pin names depend on OutputMode) |
| **Flags** (optional) | Bitmask    | Bitmask representation (when bOutputFlags is true)    |

_Output pin names vary based on OutputMode and selected enum values_

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Constants/PCGExConstantEnum.h)
