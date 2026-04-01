---
description: Wait for PCG Components Generated output.
icon: circle
---

# Wait for PCG Data

### Overview

This node waits for PCG Components on referenced actors to complete their generation, then collects their output data. It monitors actors specified via an attribute, discovers PCG Components matching a template graph, optionally triggers generation if needed, and stages the resulting data to output pins that mirror the template graph's outputs.

### How It Works

1. **Gather Actor References**: Reads actor references from an attribute on each input point, identifying which actors to monitor.
2. **Discover Components**: Searches each referenced actor for PCG Components that match the template graph and filtering criteria.
3. **Trigger Generation (optional)**: Based on each component's generation trigger type (OnLoad, OnDemand, AtRuntime), decides whether to wait, generate, or force-regenerate.
4. **Wait for Completion**: Monitors components until generation completes or timeout is reached.
5. **Stage Output Data**: Collects generated data from components and routes it to matching output pins based on pin labels from the template graph.

**Usage Notes**

* **Template Matching**: When `Must Match Template` is enabled, only components using the exact same graph instance are considered. This ensures you collect data from the expected graph.
* **Actor References**: The referenced actors must exist in the world. Use `Wait for Missing Actors` with a timeout if actors may spawn dynamically.
* **Pin Mirroring**: Output pins are created based on the template graph's output pins. Data is routed to matching pins by label.
* **Roaming Data**: Any output data that doesn't match a template pin label goes to the Roaming Data pin (if enabled).
* **Deduplication**: When enabled, each unique actor's component data is output only once, even if referenced by multiple input points.

### Behavior

```
Input Points with ActorReference attribute
    │
    ▼
┌──────────────────────────────────────┐
│  For each unique actor reference:    │
│  1. Find actor (wait if needed)      │
│  2. Find PCG Components              │
│  3. Filter by template/tags/trigger  │
│  4. Trigger generation if needed     │
│  5. Wait for generation complete     │
└──────────────────────────────────────┘
    │
    ▼
Output pins matching template graph
(+ Roaming Data pin for unmatched data)
```

### Inputs

| Pin    | Type   | Description                                             |
| ------ | ------ | ------------------------------------------------------- |
| **In** | Points | Points containing actor reference attributes to monitor |

### Settings

#### Actor & Template Settings

<details>

<summary><strong>Actor Reference Attribute</strong> <code>FName</code></summary>

The attribute name containing actor references to monitor for PCG Components.

Default: `ActorReference`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Template Input</strong> <code>EPCGExDataInputValueType</code></summary>

| Option        | Description                                         |
| ------------- | --------------------------------------------------- |
| **Constant**  | Use a fixed graph asset as the template             |
| **Attribute** | Read the template graph path from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Template Graph</strong> <code>TSoftObjectPtr&#x3C;UPCGGraph></code></summary>

The PCG Graph instance to look for on components. The node waits until a component with this graph completes generation.

📋 _Visible when Template Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Template Graph Attribute Name</strong> <code>FName</code></summary>

Attribute containing the soft object path to the template graph.

Default: `@Data.TemplateGraph`

📋 _Visible when Template Input = Attribute_

⚡ PCG Overridable

</details>

#### Filtering Settings

<details>

<summary><strong>Must Match Template</strong> <code>bool</code></summary>

When enabled, skips components whose graph instance differs from the specified template. Ensures only the expected graph's data is collected.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Must Have Tag</strong> <code>FName</code></summary>

When set, only considers PCG Components that have this component tag.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match Generation Trigger</strong> <code>bool</code></summary>

Enables filtering components by their generation trigger type.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Generation Trigger</strong> <code>EPCGComponentGenerationTrigger</code></summary>

| Option                | Description                                         |
| --------------------- | --------------------------------------------------- |
| **GenerateOnLoad**    | Match components that generate when the level loads |
| **GenerateOnDemand**  | Match components that generate on demand            |
| **GenerateAtRuntime** | Match components that generate at runtime           |

Default: `GenerateOnLoad`

📋 _Visible when Match Generation Trigger is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>└─ Invert</strong> <code>bool</code></summary>

When enabled, matches components that do NOT have the specified generation trigger.

Default: `false`

📋 _Visible when Match Generation Trigger is enabled_

⚡ PCG Overridable

</details>

#### Generation & Wait Settings

<details>

<summary><strong>Wait for Missing Actors</strong> <code>bool</code></summary>

When enabled, waits for referenced actors to exist before giving up. Useful when actors spawn dynamically.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>└─ Timeout</strong> <code>double</code></summary>

Maximum time in seconds to wait for missing actors before failing.

Default: `1`

Range: 0.001 - 30

📋 _Visible when Wait for Missing Actors is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Wait for Missing Components</strong> <code>bool</code></summary>

When enabled, waits for a matching PCG Component to appear on actors. Use carefully - only enable if you're certain a component will be added.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>└─ Timeout</strong> <code>double</code></summary>

Maximum time in seconds to wait for missing components before failing.

Default: `1`

Range: 0.001 - 30

📋 _Visible when Wait for Missing Actors is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grab GenerateOnLoad</strong> <code>EPCGExGenerationTriggerAction</code></summary>

How to handle components with GenerateOnLoad trigger.

| Option               | Description                                       |
| -------------------- | ------------------------------------------------- |
| **Ignore**           | Skip this component entirely                      |
| **As-is**            | Grab current data without triggering generation   |
| **Generate**         | Generate if not already done, wait for completion |
| **Generate (force)** | Force regeneration even if already generated      |

Default: `Generate`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grab GenerateOnDemand</strong> <code>EPCGExGenerationTriggerAction</code></summary>

How to handle components with GenerateOnDemand trigger.

| Option               | Description                                       |
| -------------------- | ------------------------------------------------- |
| **Ignore**           | Skip this component entirely                      |
| **As-is**            | Grab current data without triggering generation   |
| **Generate**         | Generate if not already done, wait for completion |
| **Generate (force)** | Force regeneration even if already generated      |

Default: `Generate`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grab GenerateAtRuntime</strong> <code>EPCGExRuntimeGenerationTriggerAction</code></summary>

How to handle components with GenerateAtRuntime trigger.

| Option      | Description                                   |
| ----------- | --------------------------------------------- |
| **Ignore**  | Skip this component entirely                  |
| **As-is**   | Grab current data without refreshing          |
| **Refresh** | Refresh the component and wait for completion |

Default: `As-is`

⚡ PCG Overridable

</details>

#### Output Settings

<details>

<summary><strong>Ignore Required Pin</strong> <code>bool</code></summary>

When enabled, outputs available data even if some required pins have no matching data. Otherwise, missing required data may cause the node to report an error.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Dedupe Data</strong> <code>bool</code></summary>

When enabled, outputs each unique actor's component data only once, even if multiple input points reference the same actor. When disabled, data is output once per reference.

Default: `true`

</details>

<details>

<summary><strong>Carry Over Target Tags</strong> <code>bool</code></summary>

When enabled, tags from the target input collection are added to the output data.

Default: `true`

</details>

<details>

<summary><strong>Target Attributes To Data Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Converts attribute values from input points into tags on the output data.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Output Roaming</strong> <code>bool</code></summary>

When enabled, data that doesn't match any template pin label is output to a separate roaming pin.

Default: `true`

</details>

<details>

<summary><strong>Roaming Pin</strong> <code>FName</code></summary>

The name of the pin that receives data not matching any template output pin.

Default: `Roaming Data`

📋 _Visible when Output Roaming is enabled_

</details>

#### Warnings and Errors

<details>

<summary><strong>Quiet Actor Not Found Warning</strong> <code>bool</code></summary>

Suppresses warnings when referenced actors are not found.

Default: `false`

</details>

<details>

<summary><strong>Quiet Component Not Found Warning</strong> <code>bool</code></summary>

Suppresses warnings when no matching PCG Components are found on actors.

Default: `false`

</details>

<details>

<summary><strong>Quiet Timeout Error</strong> <code>bool</code></summary>

Suppresses errors when waiting for components times out.

Default: `false`

</details>

### Outputs

| Pin                 | Type    | Description                                             |
| ------------------- | ------- | ------------------------------------------------------- |
| **(Template Pins)** | Various | Output pins matching the template graph's output pins   |
| **Roaming Data**    | Any     | Data that doesn't match any template pin (when enabled) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsBridges-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsBridges/Public/Elements/PCGExWaitForPCGData.h)
