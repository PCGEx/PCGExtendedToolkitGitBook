---
description: Use custom blueprint to read data from actor references.
icon: circle
---

# Pack Actor Data

### Overview

This node processes actor references stored in point attributes and extracts data from those actors using a custom Blueprint packer class. The packer defines the logic for reading actor properties, components, and other data, then writes the extracted values to point attributes. This enables complex actor-to-point data pipelines driven by custom Blueprint logic.

### How It Works

1. **Resolve References**: Reads actor references from the specified attribute on each point.
2. **Initialize Packer**: Calls the packer's Initialize function to set up attribute buffers.
3. **Process Entries**: For each valid actor reference, calls ProcessEntry with the actor and point.
4. **Write Results**: Outputs points with data extracted from the referenced actors.

**Usage Notes**

* **Custom Packer**: Create a Blueprint class extending UPCGExCustomActorDataPacker to define extraction logic.
* **Initialization**: Use Init\* functions in Initialize to declare output attributes.
* **Writing Values**: Use Write\* functions in ProcessEntry to set attribute values.
* **Main Thread**: Enable bExecuteOnMainThread if spawning components or accessing game thread resources.
* **Actor Tracking**: Enable actor tracking to regenerate when referenced actors change.

{% content-ref url="creating-a-custom-actor-data-packer.md" %}
[creating-a-custom-actor-data-packer.md](creating-a-custom-actor-data-packer.md)
{% endcontent-ref %}

### Behavior

**Actor Data Extraction:**

```
Input Points with ActorReference attribute:
   Point 0 → ActorA (Health=100, Name="Enemy")
   Point 1 → ActorB (Health=50, Name="Ally")
   Point 2 → nullptr (invalid reference)

Custom Packer extracts Health and Name:

Output Points:
   Point 0: Health=100, Name="Enemy"
   Point 1: Health=50, Name="Ally"
   (Point 2 omitted if bOmitUnresolvedEntries=true)
```

### Inputs

| Pin                    | Type   | Description                                 |
| ---------------------- | ------ | ------------------------------------------- |
| **In**                 | Points | Points with actor reference attributes      |
| **Overrides : Packer** | Params | Optional parameter overrides for the packer |

### Settings

<details>

<summary><strong>Actor Reference Attribute</strong> <code>FName</code></summary>

The attribute name containing actor references (FSoftObjectPath) to process.

Default: `ActorReference`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Packer</strong> <code>UPCGExCustomActorDataPacker</code></summary>

The custom packer instance that defines how to extract data from actors. Create a Blueprint extending UPCGExCustomActorDataPacker.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Unresolved Entries</strong> <code>bool</code></summary>

When enabled, skips points where the actor reference could not be resolved.

Default: `true`

</details>

<details>

<summary><strong>Omit Empty Outputs</strong> <code>bool</code></summary>

When enabled, skips outputting empty data collections.

Default: `true`

</details>

<details>

<summary><strong>Track Actors</strong> <code>bool</code></summary>

When enabled, tracks referenced actors and triggers graph regeneration when their properties change.

Default: `true`

</details>

#### Warning Settings

<details>

<summary><strong>Quiet Uninitialized Packer Warning</strong> <code>bool</code></summary>

Suppresses warning when the packer is not initialized.

Default: `false`

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| **Out** | Points | Points with attributes populated from actor data |

***

## Custom Actor Data Packer

The Custom Actor Data Packer is an abstract Blueprint class that defines how to extract data from actors.

### Blueprint Functions

#### Initialize

Called once at the start of processing. Use this to declare output attributes.

```cpp
void Initialize(bool& OutSuccess)
```

Available Init functions:

* `InitInt32`, `InitInt64`, `InitFloat`, `InitDouble`
* `InitVector2`, `InitVector`, `InitVector4`
* `InitQuat`, `InitTransform`, `InitRotator`
* `InitString`, `InitBool`, `InitName`
* `InitSoftObjectPath`, `InitSoftClassPath`

#### ProcessEntry

Called for each actor reference. Use this to read actor data and write to attributes.

```cpp
void ProcessEntry(AActor* InActor, const FPCGPoint& InPoint, int32 InPointIndex, FPCGPoint& OutPoint)
```

Available Write functions:

* `WriteInt32`, `WriteInt64`, `WriteFloat`, `WriteDouble`
* `WriteVector2`, `WriteVector`, `WriteVector4`
* `WriteQuat`, `WriteTransform`, `WriteRotator`
* `WriteString`, `WriteBool`, `WriteName`
* `WriteSoftObjectPath`, `WriteSoftClassPath`

Available Read functions (for reading existing point attributes):

* Corresponding Read\* functions for all types

#### Settings

<details>

<summary><strong>Execute On Main Thread</strong> <code>bool</code></summary>

Enable this if spawning components or performing operations that require the game thread.

Default: `false`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExPackActorData.h)
