---
description: Simple tag check on the input collection.
icon: circle-dashed
---

# Data Filter : Tag Check

### Overview

This filter tests whether a specified tag exists on the input data collection. It operates at the collection level, evaluating entire point sets rather than individual points. The filter supports various string matching modes and can operate in strict mode to ignore tag values when checking for tag prefixes.

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### How It Works

1. **Tag Retrieval**: Gets the tags associated with the data collection.
2. **Name Matching**: Compares the target tag name against collection tags using the selected match mode.
3. **Strict Mode**: When enabled, only checks tag prefixes and ignores values in `Tag:Value` format.
4. **Result**: Returns pass if a matching tag is found, fail otherwise.

**Usage Notes**

* **Collection Filter**: This filter evaluates at the data set level, not per-point.
* **Tag Format**: PCG tags can be simple (`MyTag`) or key-value pairs (`Category:Value`).
* **Strict Mode**: Use when you want to check for tag presence regardless of its value.

### Behavior

**Tag Check Examples:**

```
Collection tags: ["Enemy", "Spawner:Cave", "Priority:High"]

Config: Tag="Enemy", Match=Equals, Strict=false
Result: Pass (exact match)

Config: Tag="Spawner", Match=Equals, Strict=false
Result: Fail (no exact match for "Spawner")

Config: Tag="Spawner", Match=Equals, Strict=true
Result: Pass (prefix match, ignores ":Cave")

Config: Tag="Priority", Match=StartsWith, Strict=false
Result: Pass (starts with "Priority")
```

### Settings

<details>

<summary><strong>Tag Name</strong> <code>FString</code></summary>

The tag name to search for. Interpretation depends on the Match mode and Strict setting.

Default: `Tag`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match</strong> <code>EPCGExStringMatchMode</code></summary>

How to compare the tag name.

| Option         | Description                       |
| -------------- | --------------------------------- |
| **Equals**     | Exact match required              |
| **Contains**   | Tag contains the search string    |
| **StartsWith** | Tag starts with the search string |
| **EndsWith**   | Tag ends with the search string   |

Default: `Equals`

</details>

<details>

<summary><strong>Strict</strong> <code>bool</code></summary>

In strict mode, only check the tag prefix and ignore values for tags formatted as `Tag:Value`. This allows matching `Spawner:Cave` and `Spawner:Forest` with just `Spawner`.

Default: `false`

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the result of this filter. When enabled, passes become fails and vice versa.

Default: `false`

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                         | Description                   |
| ---------- | ---------------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Collection) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Collections/PCGExTagCheckFilter.h)
