---
description: >-
  Configures filtering of attributes or tags based on their names using pattern
  matching.
icon: sliders-simple
---

# Name Filters Details

### Overview

This settings block provides flexible name-based filtering for attributes, tags, or other named elements. You can include or exclude items by exact name, prefix, suffix, or substring matching. Multiple patterns can be combined for complex filtering scenarios.

### How It Works

1. **Select Mode**: Choose whether to include all, exclude matches, or include only matches
2. **Define Patterns**: Specify name patterns and how they should be matched
3. **Apply Filter**: Items are filtered based on whether their names match the defined patterns

### Behavior

```
Filter Mode Examples:

All:      [A] [B] [C] [D] → [A] [B] [C] [D]  (no filtering)

Exclude:  Pattern "temp" (Contains)
          [A] [tempB] [C] [mytemp] → [A] [C]

Include:  Pattern "pcg" (StartsWith)
          [pcgA] [B] [pcgex_C] [D] → [pcgA] [pcgex_C]
```

### Settings

<details>

<summary><strong>Filter Mode</strong> <code>EPCGExAttributeFilter</code></summary>

Determines how the filter operates on matched names.

| Option      | Description                                            |
| ----------- | ------------------------------------------------------ |
| **All**     | No filtering applied; all items pass through           |
| **Exclude** | Remove items whose names match any pattern             |
| **Include** | Keep only items whose names match at least one pattern |

Default: `All`

</details>

<details>

<summary><strong>Matches</strong> <code>TMap&#x3C;FString, EPCGExStringMatchMode></code></summary>

A map of name patterns and their matching modes. Each entry specifies a pattern string and how it should be compared against item names.

📋 _Visible when Filter Mode ≠ All_

</details>

<details>

<summary><strong>Comma Separated Names</strong> <code>FString</code></summary>

A comma-separated list of names to match. Provides a quick way to specify multiple patterns using the same match mode.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comma Separated Name Filter</strong> <code>EPCGExStringMatchMode</code></summary>

The matching mode applied to all names in the comma-separated list.

| Option          | Description                                  |
| --------------- | -------------------------------------------- |
| **Equals**      | Name must exactly match the pattern          |
| **Contains**    | Name must contain the pattern as a substring |
| **Starts With** | Name must begin with the pattern             |
| **Ends With**   | Name must end with the pattern               |

Default: `Equals`

📋 _Visible when Filter Mode ≠ All_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve PCGEx Data</strong> <code>bool</code></summary>

When enabled, internal PCGEx attributes and metadata are preserved regardless of filter settings. This prevents accidental removal of data required for PCGEx operations.

Default: `true`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Data/Utils/PCGExDataFilterDetails.h)
