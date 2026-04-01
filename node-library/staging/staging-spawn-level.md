---
description: Spawns level instances from staged points.
icon: circle
---

# Staging : Spawn Level

### Overview

This node spawns streaming level instances at the transform of each staged point. Points must carry collection mapping data from an upstream Staging node — each valid point spawns a level at its location using the referenced level asset. All spawned levels are tracked as PCG managed resources and automatically cleaned up on regeneration.

### How It Works

1. **Collection Mapping**: Reads the collection map from the Labels input pin, which provides the mapping between point hashes and level asset paths.
2. **Point Evaluation**: Each input point is checked against optional filters. Passing points have their staged entry hash resolved to a level asset path.
3. **Spawn Request Collection**: Valid points produce spawn requests containing the level path, point transform, and a unique package name.
4. **Level Spawning**: On the main thread, each request spawns either a streaming level instance or an `ALevelInstance` actor, depending on configuration.
5. **Managed Cleanup**: All spawned levels are registered with PCG's managed resource system. On re-execution, previously spawned levels are unloaded automatically.

**Usage Notes**

{% hint style="info" %}
## **Editor vs Runtime**

The Level Instance path (`ALevelInstance` actors) only works in editor. Runtime-generated PCG components automatically fall back to streaming levels.
{% endhint %}

* **bIsMainWorldOnly Filtering**: Spawned levels go through a custom streaming class that destroys actors marked `bIsMainWorldOnly`, since `LoadLevelInstance` bypasses World Partition's normal filtering.
* **CRC Reuse**: If the inputs haven't changed between executions (same CRC), previously spawned levels are reused without re-spawning.
* **Unique Naming**: Each streaming level gets a unique package name combining the suffix, a monotonic generation counter, and the point index — preventing collisions with levels still pending async unload from prior cycles.

### Inputs

| Pin               | Type                | Description                                                      |
| ----------------- | ------------------- | ---------------------------------------------------------------- |
| **Labels**        | Param               | Collection map data from upstream Staging nodes                  |
| **In**            | Points              | Staged points with collection entry hashes                       |
| **Point Filters** | Factories (Filters) | Optional filters controlling which points spawn a level instance |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Level Name Suffix</strong> <code>FString</code></summary>

Suffix appended to each spawned streaming level's package name to ensure uniqueness. If empty, uses point index.

Default: `"PCGEx"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Streaming Level Class</strong> <code>TSubclassOf&#x3C;ULevelStreamingDynamic></code></summary>

Streaming level class used for the runtime (non-LevelInstance) path. Override with a custom subclass for advanced streaming behavior. If None, defaults to `UPCGExLevelStreamingDynamic`.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Spawn As Level Instance</strong> <code>bool</code></summary>

When enabled (editor only), spawn ALevelInstance actors instead of raw streaming levels. Gives proper inspector grouping with collapsible entries in the World Outliner. Falls back to streaming if executed in cooked builds.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Level Instance Class</strong> <code>TSubclassOf&#x3C;ALevelInstance></code></summary>

Level instance actor class used for the ALevelInstance path. Override with a custom subclass for advanced level instance behavior. If None, defaults to `APCGExLevelInstance`.

Default: `None`

📋 _Visible when Spawn As Level Instance = true_

⚡ PCG Overridable

</details>

#### Warnings and Errors

<details>

<summary><strong>Quiet Runtime Fallback Warning</strong> <code>bool</code></summary>

Suppress the warning emitted when Spawn As Level Instance is enabled but the component uses Generate At Runtime (which forces a fallback to streaming levels).

Default: `false`

</details>

#### Inherited Settings

This node inherits common settings from its base class.

> See Points Processor Settings for inherited options.

### Outputs

| Pin     | Type   | Description            |
| ------- | ------ | ---------------------- |
| **Out** | Points | Forwarded input points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingLoadLevel.h)
