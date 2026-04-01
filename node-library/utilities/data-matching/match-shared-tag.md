---
description: Matches based on shared tags between datasets.
icon: circle-dashed
---

# Match : Shared Tag

### Overview

This match rule establishes correspondences between data elements based on their tags. It can match in three modes: checking for a specific named tag, matching if any tags are shared between candidate and target, or requiring all candidate tags to exist in the target. Tag matching can optionally include tag values in addition to tag names, enabling precise metadata-based correspondence.

### How It Works

1. **Mode Selection**: Chooses the tag matching strategy:
   * **Specific Tag**: Checks for a particular tag by name
   * **Any Shared**: Matches if any tag exists in both candidate and target
   * **All Shared**: Matches if all candidate tags exist in target
2. **Tag Name Determination**: For Specific mode, gets tag name from:
   * **Constant**: Fixed tag name string
   * **Attribute**: Read tag name from attribute per-element
3. **Tag Comparison**: Compares tags based on mode:
   * Name-only matching (default)
   * Name and value matching (when enabled)
4. **Value Matching**: When enabled, checks that both tag name and tag value match
5. **Strictness Application**: Applies inherited Strictness setting
6. **Inversion**: Applies inherited Invert flag if enabled

### Behavior

**Mode: Specific Tag (TagName = "Fruit")**

```
Target Tags: ["Fruit", "Red", "Sweet"]
Candidate Tags: ["Fruit", "Organic"]
→ Match ✓ (both have "Fruit")

Target Tags: ["Vegetable", "Green"]
Candidate Tags: ["Fruit", "Organic"]
→ No Match ✗ (target doesn't have "Fruit")
```

**Mode: Any Shared**

```
Target Tags: ["A", "B", "C"]
Candidate Tags: ["B", "D", "E"]
→ Match ✓ (share "B")

Target Tags: ["A", "B", "C"]
Candidate Tags: ["D", "E", "F"]
→ No Match ✗ (no shared tags)
```

**Mode: All Shared**

```
Target Tags: ["A", "B", "C", "D"]
Candidate Tags: ["B", "C"]
→ Match ✓ (all candidate tags exist in target)

Target Tags: ["A", "B"]
Candidate Tags: ["B", "C"]
→ No Match ✗ (target missing "C")
```

**Value Matching:**

```
With bDoValueMatch = true:
Target: Tag "Type" = "Apple" (Type:Apple)
Candidate: Tag "Type" = "Apple" (Type:Apple)
→ Match ✓ (name AND value match)

Target: Tag "Type" = "Apple" (Type:Apple)
Candidate: Tag "Type" = "Orange" (Type:Orange)
→ No Match ✗ (name matches but value doesn't)
```

**Attribute-Based Tag Name:**

```
Candidate[0] has TagNameAttr = "Category"
Candidate[1] has TagNameAttr = "Region"
→ Each candidate checks for its specific tag
```

Good for: metadata matching, category-based filtering, tag-based association, classification matching, hierarchical grouping

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExTagMatchMode</code></summary>

Determines the tag matching strategy.

| Option                     | Description                                                                                            |
| -------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Specific Tag** (default) | Match if a specific tag (by name) exists in both datasets. Tag name can be constant or from attribute. |
| **Any Shared**             | Match if ANY tag is shared between candidate and target. At least one tag must exist in both.          |
| **All Shared**             | Match if ALL tags from the candidate exist in the target. Target must be a superset of candidate tags. |

**Specific**: Precise control, single tag check **Any Shared**: Flexible, finds any common metadata **All Shared**: Strict, ensures complete tag subset relationship

Default: `Specific Tag`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Name Input</strong> <code>EPCGExInputValueType</code></summary>

Determines where the tag name comes from when Mode is Specific Tag.

| Option                 | Description                                                     |
| ---------------------- | --------------------------------------------------------------- |
| **Constant** (default) | Uses the fixed TagName string for all matches                   |
| **Attribute**          | Reads tag name from TagNameAttribute for per-element tag checks |

Only applies when Mode = Specific Tag.

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Name (Attribute)</strong> <code>FName</code></summary>

Attribute to read the tag name from when TagNameInput is Attribute and Mode is Specific Tag.

Examples:

* `"ReadTagFrom"` (default): Reads tag name from this attribute
* `"CategoryName"`: Each element specifies which category tag to check
* `"FilterTag"`: Per-element tag filter specification

Only used when Mode = Specific Tag and TagNameInput = Attribute.

Default: `"ReadTagFrom"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Name (Constant)</strong> <code>FString</code></summary>

Fixed tag name to check for when TagNameInput is Constant and Mode is Specific Tag.

Examples:

* `"Tag"` (default): Checks for tag named "Tag"
* `"Fruit"`: Checks for tag named "Fruit"
* `"Region_North"`: Checks for tag named "Region\_North"

Only used when Mode = Specific Tag and TagNameInput = Constant.

Default: `"Tag"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Do Value Match</strong> <code>bool</code></summary>

When enabled and Mode is Specific Tag, matches both tag name AND tag value. When disabled, only tag name is checked.

* **false** (default): Only check if tag exists (name match)
* **true**: Check if tag exists AND has matching value (name + value match)

Only applies when Mode = Specific Tag. Useful for precise metadata matching where tag values carry important information.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match Tag Values</strong> <code>bool</code></summary>

When enabled and Mode is Any Shared or All Shared, matches tag values in addition to tag names.

* **false** (default): Shared tags only need matching names
* **true**: Shared tags need matching names AND matching values

Only applies when Mode is Any Shared or All Shared. When true, a tag is only considered "shared" if both the name and value match.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

**Tip**:

* Use **Specific Tag** mode with constant tag name for simple category filtering
* Use **Any Shared** mode to find elements with overlapping metadata
* Use **All Shared** mode to find targets that encompass all candidate properties
* Enable value matching when tag values carry semantic meaning beyond just tag presence

### Outputs

| Pin            | Type       | Description                               |
| -------------- | ---------- | ----------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory for tag-based matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchSharedTag.h)
