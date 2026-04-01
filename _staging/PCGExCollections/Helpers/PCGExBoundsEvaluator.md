---
icon: arrows-left-right
description: 'Bounds Evaluator - Controls how actor bounds are computed for collections'
---

# Bounds Evaluator

Controls how actor bounds are computed for collection and exporter processing.

## Overview

Bounds Evaluators determine how an actor's world-space bounding box is calculated. The base evaluator accumulates bounds from all registered primitive components on the actor. The default implementation adds filtering options — restrict to colliding components only, exclude lights, or include child actor components — so the resulting bounds reflect only the geometry you care about.

This class is `Blueprintable` — you can create custom Blueprint subclasses with your own bounds logic.

## How It Works

1. **Gather Components**: Collects all `UPrimitiveComponent` instances on the actor (optionally including child actors).
2. **Filter Components**: Skips unregistered components, then optionally skips non-colliding components and light components based on settings.
3. **Accumulate Bounds**: Each surviving component's world-space bounds are combined into a single `FBox`. If no components contribute, the result is an invalid box.

#### Usage Notes

- **Invalid bounds**: If no components pass filtering, the evaluator returns an uninitialized `FBox`. Consuming systems should handle this as "no valid bounds."
- **Lights excluded by default**: `bIgnoreLightComponents` defaults to `true` because light volumes are typically much larger than visual geometry and would inflate bounds significantly.
- **Blueprint extensible**: Subclass `UPCGExBoundsEvaluator` in Blueprint and override `EvaluateActorBounds` for fully custom bounds logic. The owning collection and entry index are passed as context.

## Settings

### Default Bounds Evaluator

<details>
<summary><strong>Only Colliding Components</strong> <code>bool</code></summary>

When enabled, only components with collision enabled contribute to bounds. Useful when actors have non-colliding visual elements (particles, decals) that shouldn't affect the bounding box.

Default: `false`

</details>

<details>
<summary><strong>Ignore Light Components</strong> <code>bool</code></summary>

When enabled, light components are excluded from bounds computation. Light volumes are often much larger than the actor's visual geometry.

Default: `true`

</details>

<details>
<summary><strong>Include From Child Actors</strong> <code>bool</code></summary>

When enabled, components from child actor components are included in the bounds computation.

Default: `false`

</details>

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Helpers/PCGExBoundsEvaluator.h)

<!-- VERIFICATION REPORT
Node-Specific Properties: 3 documented (on UPCGExDefaultBoundsEvaluator)
Inherited Properties: Base UPCGExBoundsEvaluator has no UPROPERTYs
Inputs: N/A (helper class, not a node)
Outputs: N/A (helper class, not a node)
Nested Types: UPCGExDefaultBoundsEvaluator (documented inline)
-->
