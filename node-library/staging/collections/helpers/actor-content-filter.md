---
description: Filters which actors are eligible for collection and exporter processing.
icon: function
---

# Actor Content Filter

### Overview

Actor Content Filters control which actors pass through when collections or exporters scan a level. The base filter automatically rejects infrastructure actors — hidden actors, editor-only actors, level scripts, brushes, info actors, and navigation data — so only meaningful content remains. The default implementation adds tag-based and class-based include/exclude lists for finer control.

This class is `Blueprintable` — you can create custom Blueprint subclasses with your own filtering logic.

### How It Works

1. **Infrastructure Check**: Every actor is first tested against a built-in infrastructure filter that rejects hidden actors, editor-only actors, `ALevelScriptActor`, `AInfo`, `ABrush`, and `ANavigationData`.
2. **Include Pass**: If include tags or include classes are specified, only actors matching at least one entry in each list survive. Empty lists are permissive — they don't filter anything out.
3. **Exclude Pass**: Any actor matching an exclude tag or exclude class is rejected, regardless of include lists.

**Usage Notes**

* **Evaluation order**: Infrastructure check runs first, then tag includes, tag excludes, class includes, class excludes — in that order. An actor must pass every stage.
* **Empty include lists are permissive**: If `IncludeTags` is empty, no tag filtering is applied. Same for `IncludeClasses`. Only non-empty include lists act as allowlists.
* **Blueprint extensible**: Subclass `UPCGExActorContentFilter` in Blueprint and override `PassesFilter` to implement fully custom logic. The owning collection and entry index are passed as context.
* **Fallback behavior**: When no filter is assigned, the system falls back to the infrastructure check alone — all non-infrastructure actors pass.

### Settings

#### Default Actor Content Filter

<details>

<summary><strong>Include Tags</strong> <code>TArray&#x3C;FName></code></summary>

If non-empty, only actors with at least one of these tags pass. When empty, no tag-based inclusion filtering is applied.

Default: `empty`

</details>

<details>

<summary><strong>Exclude Tags</strong> <code>TArray&#x3C;FName></code></summary>

Actors with any of these tags are rejected.

Default: `empty`

</details>

<details>

<summary><strong>Include Classes</strong> <code>TArray&#x3C;TSoftClassPtr&#x3C;AActor>></code></summary>

If non-empty, only actors of these classes (or subclasses) pass. When empty, no class-based inclusion filtering is applied.

Default: `empty`

</details>

<details>

<summary><strong>Exclude Classes</strong> <code>TArray&#x3C;TSoftClassPtr&#x3C;AActor>></code></summary>

Actors of these classes (or subclasses) are rejected.

Default: `empty`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Helpers/PCGExActorContentFilter.h)
