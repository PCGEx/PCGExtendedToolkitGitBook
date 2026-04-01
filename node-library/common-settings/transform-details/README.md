---
description: >-
  Configures complete transform behavior including scaling, justification, and
  inheritance from target points.
icon: sliders
---

# Transform Details

{% content-ref url="justification-details.md" %}
[justification-details.md](justification-details.md)
{% endcontent-ref %}

{% content-ref url="scale-to-fit-details.md" %}
[scale-to-fit-details.md](scale-to-fit-details.md)
{% endcontent-ref %}

### Overview

This settings block provides comprehensive control over how objects are transformed when copied or placed onto target points. It combines scale-to-fit behavior, justification/alignment, and options to inherit rotation and scale from the target points. This is the primary transform configuration used by copy-to-points operations.

### How It Works

1. **Fit to Bounds**: Optionally scale the object to fit target bounds
2. **Justify Position**: Align the object within the target space
3. **Inherit Transform**: Optionally apply the target point's rotation and/or scale
4. **Final Placement**: Object is positioned with all transformations applied

### Behavior

```
Copy to Points Transform Pipeline:

Source Object → Scale to Fit → Justify → Inherit Transform → Output
     ↓              ↓            ↓              ↓
  Original      Sized to     Aligned       Rotated/Scaled
   bounds       fit target   in space      like target point
```

### Settings

<details>

<summary><strong>Scale To Fit</strong> <code>FPCGExScaleToFitDetails</code></summary>

Controls how the object is scaled to match target bounds.

→ See Scale To Fit Details

⚡ PCG Overridable

</details>

<details>

<summary><strong>Justification</strong> <code>FPCGExJustificationDetails</code></summary>

Controls how the object is aligned within the target space.

→ See Justification Details

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inherit Scale</strong> <code>bool</code></summary>

When enabled, the output inherits the scale of the target point. The object's scale is multiplied by the target point's scale.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inherit Rotation</strong> <code>bool</code></summary>

When enabled, the output inherits the rotation of the target point. The object is rotated to match the target point's orientation.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore Bounds</strong> <code>bool</code></summary>

When enabled, skips bounds-based calculations entirely. The object is placed directly at target positions without fitting or justification adjustments.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Fitting/PCGExFitting.h)
