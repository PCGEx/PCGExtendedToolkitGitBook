---
description: Routes data to different output pins based on a @Data domain attribute value.
icon: circle
---

# Branch on Data

### Overview

This control flow node reads a @Data domain attribute and routes incoming data to different output pins based on the attribute's value. It supports user-defined branch conditions (with numeric or string comparisons), enum integer matching, or enum name matching. Data that doesn't match any defined branch condition is routed to the default output pin.

### How It Works

1. **Mode Selection**: Determines how to evaluate the branch attribute (UserDefined, EnumInteger, or EnumName)
2. **Attribute Reading**: Reads the specified @Data domain attribute from each input dataset
3. **Condition Evaluation**: Compares the attribute value against defined branch conditions
4. **Output Routing**: Routes data to the matching output pin, or to the default pin if no match
5. **Pin Generation**: Creates output pins dynamically based on branch definitions or enum values

### Behavior

```
UserDefined Mode - Numeric Comparison:

Branch Attribute: @Data.Priority
Branches:
  - Label: "High", Check: Numeric, Comparison: >=, Value: 80
  - Label: "Medium", Check: Numeric, Comparison: >=, Value: 40
  - Label: "Low", Check: Numeric, Comparison: <, Value: 40

Input Data with @Data.Priority = 95 → "High" pin
Input Data with @Data.Priority = 50 → "Medium" pin
Input Data with @Data.Priority = 10 → "Low" pin
```

```
UserDefined Mode - String Comparison:

Branch Attribute: @Data.Category
Branches:
  - Label: "Trees", Check: String, Comparison: ==, Value: "Tree"
  - Label: "Rocks", Check: String, Comparison: Contains, Value: "Rock"
  - Label: "Props", Check: String, Comparison: StartsWith, Value: "Prop_"

Input Data with @Data.Category = "Tree" → "Trees" pin
Input Data with @Data.Category = "Boulder_Rock" → "Rocks" pin
Input Data with @Data.Category = "Prop_Barrel" → "Props" pin
Input Data with @Data.Category = "Unknown" → "Default" pin
```

```
EnumInteger Mode:

SelectionMode: EnumInteger
Enum: EMyBiome {Forest=0, Desert=1, Ocean=2}
Branch Attribute: @Data.BiomeID

Output Pins: "Forest", "Desert", "Ocean", "Default"

Input Data with @Data.BiomeID = 0 → "Forest" pin
Input Data with @Data.BiomeID = 1 → "Desert" pin
Input Data with @Data.BiomeID = 2 → "Ocean" pin
Input Data with @Data.BiomeID = 99 → "Default" pin
```

```
EnumName Mode:

SelectionMode: EnumName
Enum: EMyBiome {Forest, Desert, Ocean}
Branch Attribute: @Data.BiomeType

Output Pins: "Forest", "Desert", "Ocean", "Default"

Input Data with @Data.BiomeType = "Forest" → "Forest" pin
Input Data with @Data.BiomeType = "Desert" → "Desert" pin
Input Data with @Data.BiomeType = "Unknown" → "Default" pin
```

Good for: data routing, conditional workflows, category-based processing, state-based branching, pipeline switching

### Settings

<details>

<summary><strong>Branch Source</strong> <code>FName</code></summary>

The @Data domain attribute to read and compare for branching decisions.

Examples:

* `"@Data.Branch"` (default): Generic branch selector
* `"@Data.State"`: State machine routing
* `"@Data.Category"`: Category-based routing
* `"@Data.Priority"`: Priority-based routing

The attribute must exist in the @Data domain, not in point attributes.

Default: `"@Data.Branch"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Selection Mode</strong> <code>EPCGExControlFlowSelectionMode</code></summary>

Determines how branch output pins are defined and matched.

| Mode                      | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| **UserDefined** (default) | Manually define branch conditions with custom comparisons |
| **EnumInteger**           | Use enum numeric values to route data                     |
| **EnumName**              | Use enum display names (as strings) to route data         |

**UserDefined**: Full control over branch logic with numeric/string comparisons **EnumInteger**: Matches attribute value against enum integer values (0, 1, 2, etc.) **EnumName**: Matches attribute string against enum names ("Forest", "Desert", etc.)

Default: `UserDefined`

</details>

<details>

<summary><strong>Branches</strong> <code>TArray</code></summary>

User-defined branch conditions and output pin definitions.

Each branch specifies:

* **Label**: Output pin name
* **Check**: Comparison type (Numeric or String)
* **Comparison**: How to compare (==, >=, Contains, etc.)
* **Value**: The value to compare against
* **Tolerance**: For nearly-equal numeric comparisons

Branches are evaluated in order. The first matching branch determines the output pin.

📋 _Visible when Selection Mode = UserDefined_

⚡ PCG Overridable

</details>

#### Branch Definition (FPCGExBranchOnDataPin)

<details>

<summary><strong>Label</strong> <code>FName</code></summary>

The name of the output pin for this branch condition.

Each branch creates a new output pin with this name.

Default: `"None"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Check</strong> <code>EPCGExComparisonDataType</code></summary>

The type of data comparison to perform.

| Type                  | Description                |
| --------------------- | -------------------------- |
| **Numeric** (default) | Compare as numbers (int64) |
| **String**            | Compare as text (FString)  |

Determines which comparison operators and value fields are used.

Default: `Numeric`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison (Numeric)</strong> <code>EPCGExComparison</code></summary>

The comparison operator for numeric values.

| Operator                     | Symbol | Description                |
| ---------------------------- | ------ | -------------------------- |
| **Strictly Equal** (default) | ==     | Exactly equal              |
| **Strictly Not Equal**       | !=     | Not equal                  |
| **Equal Or Greater**         | >=     | Greater than or equal      |
| **Equal Or Smaller**         | <=     | Less than or equal         |
| **Strictly Greater**         | >      | Greater than               |
| **Strictly Smaller**         | <      | Less than                  |
| **Nearly Equal**             | \~=    | Equal within tolerance     |
| **Nearly Not Equal**         | !\~=   | Not equal within tolerance |

📋 _Visible when Check = Numeric_

Default: `Strictly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Value (Numeric)</strong> <code>int64</code></summary>

The numeric value to compare against when Check = Numeric.

Examples:

* `0`: Match zero
* `1`: Match one
* `100`: Match priority threshold
* `-1`: Match negative values

📋 _Visible when Check = Numeric_

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

The tolerance for nearly-equal numeric comparisons.

Values within this range are considered equal.

📋 _Visible when Check = Numeric and Comparison = Nearly Equal or Nearly Not Equal_

Default: `DBL_COMPARE_TOLERANCE` (very small value, typically 1e-8)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison (String)</strong> <code>EPCGExStringComparison</code></summary>

The comparison operator for string values.

| Operator                     | Description                        |
| ---------------------------- | ---------------------------------- |
| **Strictly Equal** (default) | Exact string match                 |
| **Strictly Not Equal**       | Not an exact match                 |
| **Length Strictly Equal**    | Same string length                 |
| **Length Strictly Unequal**  | Different string length            |
| **Length Equal Or Greater**  | String length >= comparison        |
| **Length Equal Or Smaller**  | String length <= comparison        |
| **Strictly Greater**         | String is alphabetically after     |
| **Strictly Smaller**         | String is alphabetically before    |
| **Locale Strictly Greater**  | Locale-aware alphabetically after  |
| **Locale Strictly Smaller**  | Locale-aware alphabetically before |
| **Contains**                 | String contains substring          |
| **Starts With**              | String starts with prefix          |
| **Ends With**                | String ends with suffix            |

📋 _Visible when Check = String_

Default: `Strictly Equal`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Value (String)</strong> <code>FString</code></summary>

The string value to compare against when Check = String.

Examples:

* `"Tree"`: Match "Tree" category
* `"Prop_"`: Match strings starting with "Prop\_"
* `"Rock"`: Match strings containing "Rock"

📋 _Visible when Check = String_

Default: `""` (empty string)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Enum Source</strong> <code>EPCGExEnumConstantSourceType</code></summary>

How to select the enum for EnumInteger or EnumName modes.

| Option                 | Description                              |
| ---------------------- | ---------------------------------------- |
| **Picker**             | Select enum from a dropdown picker       |
| **Selector** (default) | Use FEnumSelector for flexible selection |

📋 _Visible when Selection Mode != UserDefined_

Default: `Selector`

</details>

<details>

<summary><strong>Enum Class</strong> <code>UEnum</code></summary>

The enum class to use when Enum Source = Picker.

Select the Unreal Engine enum whose values will define the output pins.

📋 _Visible when Selection Mode != UserDefined and Enum Source = Picker_

</details>

<details>

<summary><strong>Enum Picker</strong> <code>FEnumSelector</code></summary>

The enum selector when Enum Source = Selector.

Provides a more flexible enum selection interface. Only the enum class is used internally, not the specific enum value selected here.

📋 _Visible when Selection Mode != UserDefined and Enum Source = Selector_

</details>

<details>

<summary><strong>Default Pin Name</strong> <code>FName</code></summary>

The name of the default/fallback output pin.

Data that doesn't match any branch condition is routed here. This can be customized to avoid conflicts when "Default" is a valid enum value or branch name.

Default: `"Default"`

</details>

### Inputs

| Pin    | Type | Description                                              |
| ------ | ---- | -------------------------------------------------------- |
| **In** | Any  | Input data collections to route based on @Data attribute |

### Outputs

Dynamic output pins based on Selection Mode:

* **UserDefined**: One pin per branch Label, plus Default pin
* **EnumInteger/EnumName**: One pin per enum value, plus Default pin

All output pins carry the same data type as input.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/ControlFlow/PCGExBranchOnDataAttribute.h)
