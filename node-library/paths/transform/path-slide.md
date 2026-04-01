---
description: >-
  Slide points of a path along the path, either toward the next or previous
  point.
icon: circle
---

# Path : Slide

### Overview

This node moves path points along their connecting segments toward neighboring points. Points can be slid by a relative percentage of the segment length or by an absolute distance. Original positions can be saved to an attribute and later restored.

### How It Works

1. **Determine Direction**: Based on settings, each point will slide toward its next or previous neighbor.
2. **Calculate Displacement**: Compute how far to move based on measure type (relative or discrete).
3. **Save Original Position**: Optionally store the pre-slide position in an attribute.
4. **Apply Slide**: Move each point along the path segment by the computed amount.

**Usage Notes**

* **Relative vs Discrete**: Relative mode (0.5 = halfway) adapts to segment length, producing consistent proportional movement. Discrete mode moves a fixed distance regardless of segment length.
* **Restore Mode**: Use Restore mode to undo a previous slide operation by reading positions back from the saved attribute.
* **Endpoint Behavior**: Endpoints have only one neighbor, so they can only slide in one valid direction.

### Behavior

**Slide toward Next (relative = 0.5):**

```
Before:     ●─────────●─────────●─────────●
            A         B         C         D

After:           ●────────●────────●──────●
                 A'       B'       C'     D
                 (each point slides halfway toward next)
```

**Slide toward Previous (relative = 0.25):**

```
Before:     ●─────────●─────────●─────────●

After:      ●───●────────●────────●────────
                (each slides 25% toward previous)
```

### Inputs

| Pin         | Type             | Description                                   |
| ----------- | ---------------- | --------------------------------------------- |
| **In**      | Points           | Path points to slide                          |
| **Filters** | Filter Factories | Point filters to select which points are slid |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExSlideMode</code></summary>

Whether to slide points or restore saved positions.

| Option      | Description                                                                 |
| ----------- | --------------------------------------------------------------------------- |
| **Slide**   | Move points along the path and optionally save original positions.          |
| **Restore** | Restore original positions from a saved attribute and delete the attribute. |

Default: `Slide`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>EPCGExSlideDirection</code></summary>

Which direction to slide along the path.

| Option       | Description                                  |
| ------------ | -------------------------------------------- |
| **Next**     | Slide toward the next point on the path.     |
| **Previous** | Slide toward the previous point on the path. |

Default: `Next`

📋 _Visible when Mode = Slide_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Amount Measure</strong> <code>EPCGExMeanMeasure</code></summary>

How the slide amount is interpreted.

| Option       | Description                                             |
| ------------ | ------------------------------------------------------- |
| **Relative** | Amount is a fraction of segment length (0.5 = halfway). |
| **Discrete** | Amount is an absolute distance in world units.          |

Default: `Relative`

📋 _Visible when Mode = Slide_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Slide Amount Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the slide amount value.

| Option        | Description                               |
| ------------- | ----------------------------------------- |
| **Constant**  | Use the constant Slide Amount value.      |
| **Attribute** | Read slide amount from a point attribute. |

Default: `Constant`

📋 _Visible when Mode = Slide_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Slide Amount</strong> <code>double</code></summary>

How much to slide. In Relative mode, 0.5 means halfway. In Discrete mode, this is the distance in world units.

Default: `0.5`

📋 _Visible when Mode = Slide and Slide Amount Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Old Position</strong> <code>bool</code></summary>

Store the original position in an attribute before sliding. This allows using Restore mode later to undo the slide.

Default: `true`

📋 _Visible when Mode = Slide_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Restore Position Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to write original positions to (when sliding) or read from (when restoring).

Default: `PreSlidePosition`

📋 _Visible when Write Old Position = true or Mode = Restore_

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathSlide.h)
