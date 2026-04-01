---
description: Execute multiple actions on points in priority order.
icon: circle-plus
---

# Batch Actions

### Overview

This node orchestrates the execution of multiple actions on point data. It accepts action factories from action provider nodes and executes them in priority order, allowing complex conditional operations with filter-based matching and attribute manipulation. Actions can conditionally modify point data based on filter results.

### How It Works

1. **Action Collection**: Gathers action factories from connected action provider nodes
2. **Priority Sorting**: Sorts actions by priority value (lower first, higher last)
3. **Sequential Execution**: Processes each action against all points
4. **Filter Evaluation**: Each action's filters determine which points are affected
5. **Conditional Operations**: Actions perform operations based on match success/fail results
6. **Attribute Cleanup**: Optionally removes processed attributes after completion

**Usage Notes**

* **Action Pipeline**: Actions are executed in sequence, with later actions seeing results of earlier ones
* **Filter Integration**: Each action can have associated filters to control which points it affects
* **Default Attributes**: Provides fallback attribute filtering for actions that don't specify their own
* **Cleanup**: Can automatically remove temporary attributes used during processing

### Inputs

| Pin         | Type             | Description                           |
| ----------- | ---------------- | ------------------------------------- |
| **In**      | Point Data       | Points to process with actions        |
| **Actions** | Action Factories | Action providers to execute on points |

### Outputs

| Pin     | Type       | Description                    |
| ------- | ---------- | ------------------------------ |
| **Out** | Point Data | Points after action processing |

### Settings

#### Default Configuration

<details>

<summary><strong>Default Attributes Filter</strong> <code>FPCGExAttributeGatherDetails</code></summary>

Default attribute filtering configuration used by actions that don't specify their own attribute filters. Controls which attributes are forwarded or processed by default. Includes:

Default: All attributes

//→ See TODO FPCGExAttributeGatherDetails

</details>

#### Attribute Cleanup

<details>

<summary><strong>Do Consume Processed Attributes</strong> <code>bool</code></summary>

Enable removal of processed attributes after all actions complete. Useful for cleaning up temporary attributes used during action processing.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Consume Processed Attributes</strong> <code>FPCGExNameFiltersDetails</code></summary>

Specifies which attributes to remove after processing when Do Consume Processed Attributes is enabled. Uses name pattern matching to identify attributes for removal.

Default: Empty (no consumption)

📋 _Visible when Do Consume Processed Attributes is enabled_

//→ See TODO FPCGExNameFiltersDetails

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsActions-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsActions/Public/Elements/PCGExBatchActions.h)
