---
description: Checks gameplay tags of an actor reference.
icon: circle-dashed
---

# Filter : GameplayTags

### Overview

This filter evaluates points by querying the gameplay tags of actors referenced through an attribute. It reads an actor reference attribute (typically from a "Get Actor Data" node), follows a property path to locate a gameplay tag container on that actor, and tests it against a gameplay tag query. This enables filtering based on actor-level tag systems without requiring the tags to be stored directly as point attributes.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Actor Resolution**: Reads the actor reference from the specified attribute for each point.
2. **Property Navigation**: Follows the property path from the actor to locate the tag container.
3. **Query Evaluation**: Runs the gameplay tag query against the found tag container.
4. **Fallback Handling**: Returns configured fallback values if the actor or property cannot be resolved.

**Usage Notes**

* **Actor Reference**: Works with FSoftObjectPath attributes, typically from "Get Actor Data" nodes.
* **Property Path**: Uses dot notation to navigate from actor root to the tag container (e.g., "Component.TagContainer").
* **Query Syntax**: Uses standard Unreal Gameplay Tag Query format with all its operators.

### Behavior

**Property Path Resolution:**

```
Actor Reference → MyCharacterActor
Property Path  → "CharacterComponent.GameplayTags"

Resolution:
1. Get actor from reference
2. Find "CharacterComponent" on actor
3. Access "GameplayTags" property on component
4. Run query against that container
```

**Query Examples:**

```
Query: "Character.Player" (any match)
Actor Tags: [Character.Player, Ability.Jump]
Result: Pass

Query: "Character.Enemy" (any match)
Actor Tags: [Character.Player, Ability.Jump]
Result: Fail
```

### Settings

<details>

<summary><strong>Actor Reference</strong> <code>FName</code></summary>

Name of the attribute that contains a path to an actor in the level. This is typically from a "Get Actor Data" PCG node operating in point mode.

Default: `ActorReference`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Property Path</strong> <code>FString</code></summary>

Path to the tag container to be tested, resolved from the actor reference as root. Uses dot notation to navigate through components and properties.

Default: `Component.TagContainer`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tag Query</strong> <code>FGameplayTagQuery</code></summary>

The gameplay tag query to evaluate against the resolved tag container. Uses standard Unreal Gameplay Tag Query syntax with support for all operators (Any, All, None, etc.).

</details>

<details>

<summary><strong>Fallback Missing Actor</strong> <code>bool</code></summary>

Value the filter returns for points whose actor reference cannot be resolved.

Default: `false`

</details>

<details>

<summary><strong>Fallback Property Path</strong> <code>bool</code></summary>

Value the filter returns if the actor is found but the property path could not be resolved.

Default: `false`

</details>

<details>

<summary><strong>Quiet Missing Property Warning</strong> <code>bool</code></summary>

Suppress warnings when the property path cannot be resolved on an actor.

Default: `false`

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExGameplayTagsFilter.h)
