---
description: Copies tags from matched targets to candidates.
icon: circle-dashed
---

# Match : Copy Tags

### Overview

This match rule not only establishes matches but also performs a side effect: when a match is found, it copies all tags from the matched target element to the candidate element. This is useful for propagating metadata, categories, or classification information from one dataset to another during matching operations. Unlike other match rules that only test for correspondence, this rule actively modifies the candidate data by inheriting the target's tags.

### How It Works

1. **Match Evaluation**: Tests whether the candidate and target should match (based on the matching context)
2. **Tag Identification**: If a match is found, identifies all tags present on the target element
3. **Tag Copying**: Copies all target tags to the candidate element
4. **Strictness Application**: Applies the inherited Strictness setting
5. **Inversion**: Applies the inherited Invert flag if enabled

The key behavior is that this rule has a side effect - it modifies candidate data by copying tags, rather than just evaluating a match condition.

### Behavior

**Match and Copy Example:**

```
Before Match:
  Target[0]:    Tags = ["Fruit", "Red", "Sweet"]
  Candidate[0]: Tags = ["Organic"]

After Successful Match:
  Target[0]:    Tags = ["Fruit", "Red", "Sweet"]  (unchanged)
  Candidate[0]: Tags = ["Organic", "Fruit", "Red", "Sweet"]

If No Match:
  Candidate retains original tags only
```

**Strictness (Any vs All):**

```
When used in contexts with multiple potential matches:
- Any: First successful match copies its tags
- All: All matched targets' tags are copied (accumulates)
```

**Invert Behavior:**

```
With bInvert = true:
- Tags are copied when the match would normally FAIL
- Inverts the copying logic
```

Good for: tag propagation, metadata inheritance, classification transfer, attribute copying during matching

### Settings

#### Node-Specific Settings

This match rule has no additional node-specific settings beyond the inherited base settings. All configuration comes from the Strictness and Invert properties.

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

**Important Notes:**

* This rule has a side effect: it modifies candidate data by copying tags
* Tag copying happens during match evaluation, not as a separate post-process
* Tags accumulate if multiple matches occur
* Original candidate tags are preserved (tags are added, not replaced)

**Tip**: Use this rule when you need to transfer categorical or classification information from a reference dataset to your working dataset during matching operations. It's particularly useful for enriching point data with metadata from a lookup table.

### Outputs

| Pin            | Type       | Description                                         |
| -------------- | ---------- | --------------------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory that copies tags during matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchCopyTags.h)
