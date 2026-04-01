---
description: Spawns actors from staged collection entries.
icon: circle
---

# Staging : Spawn Actors

### Overview

This node spawns actor instances at the transform of each staged point. Points must carry collection mapping data from an upstream Staging node — each valid point resolves to an actor class from an Actor Collection and spawns it at the point's location. Spawned actors are tracked as PCG managed resources and automatically cleaned up on regeneration.

### How It Works

1. **Collection Mapping**: Reads the collection map from the Labels input pin, which provides the mapping between point hashes and actor collection entries.
2. **Point Resolution**: Each input point is checked against optional filters. Passing points have their staged entry hash resolved to an actor collection entry. Only entries of type Actor are accepted.
3. **Class Loading**: All unique actor classes across resolved entries are batch-loaded asynchronously before any spawning begins.
4. **Actor Spawning**: On the main thread, each resolved point spawns its actor at the point's transform. Optionally, collection entry tags are applied to the spawned actor.
5. **PCG Generation**: If enabled, spawned actors with PCG components can have their generation triggered automatically based on configurable rules per trigger type.
6. **Output Reference**: Each spawned actor's soft object path is written to a configurable attribute on the output points.

**Usage Notes**

* **Actor Collections Only**: This node only works with Actor Collection entries. Non-actor collection entries are skipped with a warning (suppressible).
* **CRC Reuse**: If the inputs haven't changed between executions (same CRC), previously spawned actors are reused without re-spawning, and actor references are written from the existing managed resource.
* **Transform Consumption**: Transforms are used as-is from upstream staging nodes — fitting and placement adjustments are the responsibility of upstream nodes.
* **PCG Generation Watcher**: When triggering PCG generation on spawned actors, the node <mark style="color:$warning;">watches and waits for completion</mark>. Each trigger type (_GenerateOnLoad, GenerateOnDemand, GenerateAtRuntime_) can be handled independently.

### Inputs

| Pin               | Type                | Description                                              |
| ----------------- | ------------------- | -------------------------------------------------------- |
| **Labels**        | Param               | Collection map data from upstream Staging nodes          |
| **In**            | Points              | Staged points with collection entry hashes               |
| **Point Filters** | Factories (Filters) | Optional filters controlling which points spawn an actor |

### Settings

#### Spawning

<details>

<summary><strong>Collision Handling</strong> <code>ESpawnActorCollisionHandlingMethod</code></summary>

How to handle collisions when spawning actors.

Default: `AlwaysSpawn`

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Apply Entry Tags</strong> <code>bool</code></summary>

If enabled, apply collection entry tags to spawned actors.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Targets Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Attribute forwarding from input points to output points.

⚡ PCG Overridable

→ See Forward Details for details.

</details>

#### PCG Generation

<details>

<summary><strong>Trigger PCG Generation</strong> <code>bool</code></summary>

If enabled, trigger PCG generation on spawned actors that have PCG components.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grab GenerateOnLoad</strong> <code>EPCGExGenerationTriggerAction</code></summary>

How to deal with found components that have the trigger condition GenerateOnLoad.

| Option            | Description                                   |
| ----------------- | --------------------------------------------- |
| **Ignore**        | Skip these components entirely                |
| **As-is**         | Leave the component's current state unchanged |
| **Generate**      | Trigger generation                            |
| **ForceGenerate** | Force generation even if already generated    |

Default: `Generate`

📋 _Visible when Trigger PCG Generation = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grab GenerateOnDemand</strong> <code>EPCGExGenerationTriggerAction</code></summary>

How to deal with found components that have the trigger condition GenerateOnDemand.

| Option            | Description                                   |
| ----------------- | --------------------------------------------- |
| **Ignore**        | Skip these components entirely                |
| **As-is**         | Leave the component's current state unchanged |
| **Generate**      | Trigger generation                            |
| **ForceGenerate** | Force generation even if already generated    |

Default: `Generate`

📋 _Visible when Trigger PCG Generation = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grab GenerateAtRuntime</strong> <code>EPCGExRuntimeGenerationTriggerAction</code></summary>

How to deal with found components that have the trigger condition GenerateAtRuntime.

| Option      | Description                                   |
| ----------- | --------------------------------------------- |
| **Ignore**  | Skip these components entirely                |
| **As-is**   | Leave the component's current state unchanged |
| **Refresh** | Refresh the component                         |

Default: `As-is`

📋 _Visible when Trigger PCG Generation = true_

⚡ PCG Overridable

</details>

#### Output

<details>

<summary><strong>Actor Reference Attribute</strong> <code>FName</code></summary>

Name of the attribute to write the spawned actor's soft object path to on each output point.

Default: `"ActorReference"`

⚡ PCG Overridable

</details>

#### Warnings and Errors

<details>

<summary><strong>Quiet Invalid Entry Warnings</strong> <code>bool</code></summary>

Suppress warnings for invalid collection entries (non-actor types, failed loads, failed spawns).

Default: `false`

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Points Processor Settings for inherited options.

### Outputs

| Pin     | Type   | Description                                                             |
| ------- | ------ | ----------------------------------------------------------------------- |
| **Out** | Points | Input points with actor reference attributes written for spawned actors |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingSpawnActors.h)
