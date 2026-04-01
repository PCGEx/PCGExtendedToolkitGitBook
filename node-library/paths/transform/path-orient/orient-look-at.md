---
description: Orients points to look at a target point, direction, or position.
icon: function
---

# Orient Look At

### Overview

This orientation method rotates points so their forward axis points toward a specified target. The target can be a neighboring path point, a direction vector from an attribute, or a world position from an attribute.

### How It Works

1. **Determine Target**: Based on the selected mode, get either a neighboring point position, a direction vector, or a world position.
2. **Compute Direction**: Calculate the direction from the current point to the target (or use the direction directly).
3. **Build Transform**: Construct the orientation with the look-at direction as the forward axis and the configured up axis.

**Usage Notes**

* **Path Neighbors**: Next Point and Previous Point modes use the path topology to find targets. At path endpoints, the available neighbor is used.
* **Attribute Modes**: Direction and Position modes read from point attributes, allowing per-point control over orientation targets.
* **Direction vs Position**: Direction mode uses the attribute value directly as a direction vector. Position mode computes the direction from the point's location to the attribute position.

### Behavior

```
Look At Modes:

Next Point:              Previous Point:
  Current ●────→ Next      Previous ←────● Current

Direction (attribute):   Position (attribute):
  Current ●────→ Dir         Current ●────→ Target Position
            (from attr)                    (world space)
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Look At</strong> <code>EPCGExOrientLookAtMode</code></summary>

Determines what the point should look at.

| Option             | Description                                                 |
| ------------------ | ----------------------------------------------------------- |
| **Next Point**     | Look toward the next point in the path.                     |
| **Previous Point** | Look toward the previous point in the path.                 |
| **Direction**      | Use a vector attribute as the look direction (local space). |
| **Position**       | Use a vector attribute as a world position to look at.      |

Default: `NextPoint`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Look At Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The vector attribute to read the look-at direction or position from.

📋 _Visible when Look At = Direction or Position_

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Orient Operation for: Orient Axis, Up Axis, and other base orientation settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Orient/PCGExOrientLookAt.h)
