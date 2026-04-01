---
icon: cog
description: 'Staging : Spawn Actors - Spawns actors from staged collection entries.'
---

# Staging : Spawn Actors

Spawns actors from staged collection entries.

## Overview

Staging : Spawn Actors reads a collection map produced by upstream staging nodes, resolves each point's entry to an actor class, and spawns that actor at the point's transform. Spawned actors can receive collection entry tags, per-instance tags, property overrides, and PCG generation triggers. Each spawned actor reference is written back to the output points for downstream use.

## How It Works

1. **Read Collection Map**: Loads the collection map from the input pin, which maps entry hashes on each point to actor collection entries
2. **Resolve Entries**: For each input point that passes filters, looks up the entry hash and resolves it to an `FPCGExActorCollectionEntry`. Non-actor entries are skipped.
3. **Batch-Load Classes**: Collects all unique actor class paths and loads them asynchronously via streaming before any spawning begins
4. **Spawn Actors**: On the main thread, spawns each resolved actor at the point's transform. When property deltas are enabled, actors are spawned deferred so properties are set before construction completes.
5. **Apply Tags & Properties**: Optionally applies collection entry tags, per-instance tags from the `InstanceTags` attribute, and serialized property deltas
6. **Trigger PCG Generation**: If enabled, finds PCG components on spawned actors and triggers generation according to the configured trigger actions
7. **Write References**: Writes a soft object path to each spawned actor into the configured output attribute
8. **Register Managed Resources**: Registers all spawned actors with PCG's managed resource system for automatic cleanup. CRC-based reuse skips spawning entirely when inputs haven't changed.

#### Usage Notes

- **Main Thread Only**: Actor spawning requires the main thread. This node cannot be parallelized.
- **Upstream Transforms**: Transforms are consumed as-is from upstream staging nodes — fitting and placement are their responsibility, not this node's.
- **CRC Reuse**: When inputs and settings haven't changed between executions, previously spawned actors are reused without respawning.
- **Deferred Construction**: When Apply Property Deltas is enabled, actors are spawned deferred. Properties are applied before construction finishes, which is important for actors whose construction scripts depend on those values.
- **Editor Folders**: In the editor, spawned actors are organized under a `<OwnerName>_Generated` folder for tidiness.

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **In** | Points | Input points with staged entry hashes from upstream staging nodes |
| **Collection Map** | Params | Collection map data from staging nodes (required) |
| **Point Filters** | Factories | Optional point filters controlling which points spawn an actor |

## Settings

### Spawning

<details>
<summary><strong>Collision Handling</strong> <code>ESpawnActorCollisionHandlingMethod</code></summary>

How to handle collisions when spawning actors.

Default: `AlwaysSpawn`

⚡ PCG Overridable

</details>

### Tagging

<details>
<summary><strong>Apply Entry Tags</strong> <code>bool</code></summary>

If enabled, applies tags from the resolved collection entry to each spawned actor.

Default: `false`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Targets Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Attribute forwarding from input points to output points.

⚡ PCG Overridable

</details>

<details>
<summary><strong>Apply Instance Tags</strong> <code>bool</code></summary>

If enabled, reads a comma-separated `InstanceTags` string attribute from each point and applies those tags to the spawned actor.

Default: `false`

⚡ PCG Overridable

</details>

### Properties

<details>
<summary><strong>Apply Property Deltas</strong> <code>bool</code></summary>

If enabled, applies per-instance property deltas stored on actor collection entries. Actors with deltas are spawned deferred so properties are set before construction completes.

Default: `false`

⚡ PCG Overridable

</details>

### PCG Generation

<details>
<summary><strong>Trigger PCG Generation</strong> <code>bool</code></summary>

If enabled, triggers PCG generation on spawned actors that have PCG components. The trigger actions below control how each component's generation trigger condition is handled.

Default: `false`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Grab GenerateOnLoad</strong> <code>EPCGExGenerationTriggerAction</code></summary>

How to handle PCG components that have the `GenerateOnLoad` trigger condition.

| Option | Description |
|--------|-------------|
| **Ignore** | Skip these components |
| **As-is** | Leave the component's existing trigger unchanged |
| **Generate** | Trigger generation |
| **ForceGenerate** | Force trigger generation |

Default: `Generate`

⚡ PCG Overridable

📋 *Visible when Trigger PCG Generation is enabled*

</details>

<details>
<summary><strong>Grab GenerateOnDemand</strong> <code>EPCGExGenerationTriggerAction</code></summary>

How to handle PCG components that have the `GenerateOnDemand` trigger condition.

| Option | Description |
|--------|-------------|
| **Ignore** | Skip these components |
| **As-is** | Leave the component's existing trigger unchanged |
| **Generate** | Trigger generation |
| **ForceGenerate** | Force trigger generation |

Default: `Generate`

⚡ PCG Overridable

📋 *Visible when Trigger PCG Generation is enabled*

</details>

<details>
<summary><strong>Grab GenerateAtRuntime</strong> <code>EPCGExRuntimeGenerationTriggerAction</code></summary>

How to handle PCG components that have the `GenerateAtRuntime` trigger condition.

| Option | Description |
|--------|-------------|
| **Ignore** | Skip these components |
| **As-is** | Leave the component's existing trigger unchanged |
| **Refresh** | Refresh generation at runtime |

Default: `As-is`

⚡ PCG Overridable

📋 *Visible when Trigger PCG Generation is enabled*

</details>

### Output

<details>
<summary><strong>Actor Reference Attribute</strong> <code>FName</code></summary>

Name of the attribute to write the spawned actor's soft object path to. Each point that successfully spawns an actor gets this attribute populated.

Default: `"ActorReference"`

⚡ PCG Overridable

</details>

### Warnings and Errors

<details>
<summary><strong>Quiet Invalid Entry Warnings</strong> <code>bool</code></summary>

Suppresses warnings for invalid collection entries (missing actor classes, non-actor entries, failed spawns).

Default: `false`

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Points Processor Settings](../../node-library/common-settings/points-processor-settings.md) for shared point processing options.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Out** | Points | Input points with the actor reference attribute written for successfully spawned actors |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingSpawnActors.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 11 documented (CollisionHandling, bApplyEntryTags, TargetsForwarding, bApplyInstanceTags, bApplyPropertyDeltas, bTriggerPCGGeneration, GenerateOnLoadAction, GenerateOnDemandAction, GenerateAtRuntimeAction, ActorReferenceAttribute, bQuietInvalidEntryWarnings)
Inherited Properties: Referenced to UPCGExPointsProcessorSettings
Inputs: In (Points), Collection Map (Params), Point Filters (Factories)
Outputs: Out (Points)
Nested Types: EPCGExGenerationTriggerAction, EPCGExRuntimeGenerationTriggerAction documented inline
-->
