---
description: >-
  Configures how spawned components or actors attach to their parent in terms of
  transform behavior.
icon: sliders-simple
---

# Attachment Rules

### Overview

This settings block controls the attachment behavior when PCGEx spawns components like splines or dynamic meshes. Attachment rules determine whether the spawned object keeps its world-space transform, snaps to the parent's transform, or maintains its relative offset. These settings mirror Unreal Engine's standard attachment system.

### How It Works

1. **Spawn Component**: A new component (spline, mesh, etc.) is created
2. **Apply Rules**: Each transform component (location, rotation, scale) follows its rule
3. **Attach to Parent**: Component is attached with the specified behavior

### Behavior

```
Attachment Rules:

KeepRelative: Maintains offset relative to parent
  Parent moves → Child moves with it, keeping offset

KeepWorld: Preserves world-space transform
  Parent moves → Child stays at world position

SnapToTarget: Matches parent transform exactly
  Child snaps to parent's location/rotation/scale
```

### Settings

<details>

<summary><strong>Location Rule</strong> <code>EAttachmentRule</code></summary>

Determines how the component's location behaves when attached.

| Option             | Description                               |
| ------------------ | ----------------------------------------- |
| **Keep Relative**  | Maintains the relative offset from parent |
| **Keep World**     | Preserves the world-space location        |
| **Snap to Target** | Snaps to the parent's location            |

Default: `KeepWorld`

</details>

<details>

<summary><strong>Rotation Rule</strong> <code>EAttachmentRule</code></summary>

Determines how the component's rotation behaves when attached.

| Option             | Description                                 |
| ------------------ | ------------------------------------------- |
| **Keep Relative**  | Maintains the relative rotation from parent |
| **Keep World**     | Preserves the world-space rotation          |
| **Snap to Target** | Snaps to the parent's rotation              |

Default: `KeepWorld`

</details>

<details>

<summary><strong>Scale Rule</strong> <code>EAttachmentRule</code></summary>

Determines how the component's scale behaves when attached.

| Option             | Description                              |
| ------------------ | ---------------------------------------- |
| **Keep Relative**  | Maintains the relative scale from parent |
| **Keep World**     | Preserves the world-space scale          |
| **Snap to Target** | Snaps to the parent's scale              |

Default: `KeepWorld`

</details>

<details>

<summary><strong>Weld Simulated Bodies</strong> <code>bool</code></summary>

When enabled, physics-simulated bodies are welded together upon attachment. This creates a single combined physics body rather than separate simulating objects.

Default: `false`

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Details/PCGExAttachmentRules.h)
