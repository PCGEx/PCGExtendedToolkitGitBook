---
description: Forward attributes based on match result.
icon: circle-dashed
---

# Action : Write Attributes

### Overview

This action factory conditionally forwards attributes from input attribute sets to points based on filter match results. When a filter matches successfully, it forwards one set of attributes; when the filter fails, it forwards a different set. This allows conditional attribute writing within the Batch Actions workflow.

### How It Works

1. **Match Evaluation**: The parent filter determines whether each point matches
2. **Success Path**: On successful match, forwards attributes from the success attribute set
3. **Fail Path**: On failed match, forwards attributes from the fail attribute set
4. **Attribute Writing**: Writes the selected attributes to the corresponding points

**Usage Notes**

* **Batch Actions Integration**: This action is used within the Batch Actions node, not standalone
* **Attribute Sets**: Requires attribute set inputs containing the values to forward
* **Conditional Logic**: Enables different attribute values based on filter results
* **Attribute Filtering**: Both success and fail paths support attribute filtering patterns

### Inputs

When used in Batch Actions:

| Pin              | Type          | Description                                      |
| ---------------- | ------------- | ------------------------------------------------ |
| **MatchSuccess** | Attribute Set | Attributes to forward on successful filter match |
| **MatchFail**    | Attribute Set | Attributes to forward on failed filter match     |

### Settings

#### Success Attributes

<details>

<summary><strong>Success Attributes Filter</strong> <code>FPCGExAttributeGatherDetails</code></summary>

Controls which attributes from the MatchSuccess input are forwarded to points when the filter matches. Supports include/exclude patterns and attribute name filtering.

Default: All attributes

//→ See TODO FPCGExAttributeGatherDetails

</details>

#### Fail Attributes

<details>

<summary><strong>Fail Attributes Filter</strong> <code>FPCGExAttributeGatherDetails</code></summary>

Controls which attributes from the MatchFail input are forwarded to points when the filter fails. Supports include/exclude patterns and attribute name filtering.

Default: All attributes

//→ See TODO FPCGExAttributeGatherDetails

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsActions-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsActions/Public/Actions/PCGExActionWriteValues.h)
