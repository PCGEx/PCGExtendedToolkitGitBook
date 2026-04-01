---
description: Destroys actors previously spawned by PCG based on actor reference attributes.
icon: circle
---

# Destroy Actor

### Overview

This node destroys actors that were previously spawned by the PCG component executing the graph. It reads actor reference attributes from point data and queues those actors for destruction. This is primarily used for cleanup operations when regenerating procedural content or removing specific spawned instances.

### How It Works

1. **Attribute Reading**: Reads actor reference from specified attribute on each point
2. **Reference Validation**: Checks that actor was spawned by the current PCG component
3. **Destruction Queuing**: Marks valid actors for destruction
4. **Cleanup Execution**: Destroys queued actors
5. **Output**: Passes through input point data unchanged

**Usage Notes**

* **Safety**: This node only destroys actors that were spawned by the PCG component currently executing the graph. Manually-placed actors or actors from other PCG components are protected from accidental deletion.
* **Regeneration Workflow**: Commonly used when regenerating procedural content - destroy old actors before spawning new ones.
* **References Become Invalid**: After destruction, the actor reference attributes in the output data point to destroyed actors. These references should not be used after this node.
* **Async Execution**: Actor destruction happens asynchronously on the main thread for thread safety.

### Behavior

**Basic Destruction:**

```
Input: 10 points with ActorReference attribute

Point 0: ActorReference → Actor_Tree_001
Point 1: ActorReference → Actor_Rock_034
Point 2: ActorReference → Actor_Tree_002
...

Result:
- Actor_Tree_001 destroyed
- Actor_Rock_034 destroyed
- Actor_Tree_002 destroyed
- etc.

Output: Same 10 points (references now invalid)
```

**Safety - PCG Component Ownership:**

```
Actor spawned by PCG Component A:
  Can be destroyed by nodes in Component A ✓
  Cannot be destroyed by nodes in Component B ✗

This prevents accidental destruction of actors from other systems
```

**Multiple References:**

```
Point 0: ActorReference → Actor_001
Point 1: ActorReference → Actor_001 (same actor)
Point 2: ActorReference → Actor_002

Result:
- Actor_001 destroyed once (duplicate references handled)
- Actor_002 destroyed
```

**Missing or Invalid References:**

```
Point with invalid reference:
  - Reference to non-existent actor → Skipped
  - Reference to null → Skipped
  - Reference to manually-placed actor → Not destroyed (safety)

No errors, invalid references silently ignored
```

Good for: cleanup, regeneration, instance removal, procedural content management

### Settings

<details>

<summary><strong>Actor Reference Attribute</strong> <code>FName</code></summary>

Name of the point attribute containing actor references.

This attribute should be of type `SoftObjectPath` or similar actor reference type, typically created by PCG spawning nodes.

Examples:

* `"ActorReference"` (default): Standard PCG actor reference attribute
* `"SpawnedActor"`: Custom reference attribute name
* `"InstanceRef"`: Alternative naming

The attribute must contain valid references to actors spawned by the same PCG component.

Default: `"ActorReference"`

⚡ PCG Overridable

</details>

### Practical Examples

**Cleanup Before Regeneration:**

```
[Old spawn data with ActorReference]
  ↓
[Destroy Actor] → Removes old spawned actors
  ↓
[New Spawn Actors] → Creates fresh actors
```

**Selective Removal:**

```
[Filter points by condition]
  ↓ Inside (to remove)
[Destroy Actor] → Destroys filtered instances
  ↓ Outside (to keep)
[Keep existing actors]
```

**Refresh Specific Instances:**

```
[Points tagged "NeedsRefresh"]
  ↓
[Destroy Actor] → Remove outdated instances
  ↓
[Spawn Actors] → Create updated instances
```

### Inputs

| Pin    | Type       | Description                            |
| ------ | ---------- | -------------------------------------- |
| **In** | Point Data | Points with actor reference attributes |

### Outputs

| Pin     | Type       | Description                                           |
| ------- | ---------- | ----------------------------------------------------- |
| **Out** | Point Data | Pass-through of input points (references now invalid) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Utils/PCGExDestroyActor.h)
