---
description: Configures how point attribute values are converted into data-level tags.
icon: sliders-simple
---

# Attribute To Tag Details

### Overview

This settings block transforms attribute values into tags that can be used for data identification and filtering. By promoting attribute values to tags, you can leverage PCG's tag-based routing and filtering systems with data derived from point attributes. This is particularly useful when copying or transforming data and needing to preserve identification metadata.

### How It Works

1. **Select Attributes**: Specify which attributes to convert
2. **Format Tags**: Optionally prefix tags with attribute names for clarity
3. **Generate Tags**: Each attribute value becomes a tag on the output data

### Behavior

```
Source Point with Attributes:
  BiomeType = "Forest"
  Priority = 5

With Prefix Enabled:
  Tags: ["BiomeType:Forest", "Priority:5"]

Without Prefix:
  Tags: ["Forest", "5"]

With Index Tag (prefix "Idx"):
  Tags: [..., "Idx:0", "Idx:1", "Idx:2"]
```

### Settings

<details>

<summary><strong>Add Index Tag</strong> <code>bool</code></summary>

When enabled, adds a tag containing the point's index within its collection.

Default: `false`

</details>

<details>

<summary><strong>Index Tag Prefix</strong> <code>FString</code></summary>

The prefix used for index tags when Add Index Tag is enabled.

Default: `IndexTag`

📋 _Visible when Add Index Tag = true_

</details>

<details>

<summary><strong>Prefix With Attribute Name</strong> <code>bool</code></summary>

When enabled, tags are prefixed with the source attribute name (e.g., "AttributeName:Value"). When disabled, only the value is used as the tag.

Default: `true`

</details>

<details>

<summary><strong>Attributes</strong> <code>TArray&#x3C;FPCGAttributePropertyInputSelector></code></summary>

List of attribute selectors specifying which attributes to convert to tags. Each selected attribute's value will become a tag on the output data.

</details>

<details>

<summary><strong>Comma Separated Attribute Selectors</strong> <code>FString</code></summary>

A comma-separated list of attribute names to convert to tags. Provides a quick way to specify multiple attributes without using the array.

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Data/Utils/PCGExDataForwardDetails.h)
