---
description: Create spline mesh components from paths using asset collections.
icon: circle
---

# Staging : Spline Mesh

### Overview

This node generates Spline Mesh Components along paths, using meshes from asset collections. It supports both staged points (from Asset Staging node) and direct collection assignment, with extensive configuration for tangents, fitting, justification, material distribution, and component properties. The node creates actual runtime spline mesh components attached to a target actor.

### How It Works

1. **Mesh Selection**: Selects meshes for each path segment from collections (staged or direct)
2. **Tangent Calculation**: Computes or reads tangent vectors for spline mesh start/end points
3. **Fitting & Justification**: Applies scale-to-fit and justification transformations if not using staged data
4. **Segment Creation**: Creates spline mesh components for each path segment with computed parameters
5. **Component Configuration**: Applies mesh, materials, descriptors, and property overrides to components

**Usage Notes**

* **Spawner Node**: This creates actual runtime components, not point data - it's a spawner type node
* **Target Actor**: Components are attached to the specified target actor (or a generated actor if none specified)
* **Staged vs Direct**: Use staged points for consistent asset management workflow, or assign collections directly
* **Tangent Modes**: Supports constant, attribute-based, or factory-generated tangents for spline curvature
* **Material Distribution**: For mesh collections with material variants, controls how materials are selected per segment

### Inputs

| Pin    | Type      | Description                                |
| ------ | --------- | ------------------------------------------ |
| **In** | Path Data | Paths to create spline mesh segments along |

### Outputs

| Pin     | Type      | Description                                             |
| ------- | --------- | ------------------------------------------------------- |
| **Out** | Path Data | Original paths (components are created as side effects) |

### Settings

#### Collection Configuration

<details>

<summary><strong>Use Staged Points</strong> <code>bool</code></summary>

Use collection map references from staged points rather than directly assigning collections. When enabled, expects points to have been staged by the Asset Staging node.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Collection Source</strong> <code>EPCGExCollectionSource</code></summary>

Where to load the mesh collection from when not using staged points.

| Option             | Description                                   |
| ------------------ | --------------------------------------------- |
| **Asset**          | Use a single collection asset                 |
| **Attribute Set**  | Build collection from attribute set           |
| **Path Attribute** | Read collection path from per-point attribute |

Default: `Asset`

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false_

</details>

<details>

<summary><strong>Asset Collection</strong> <code>TSoftObjectPtr</code></summary>

The Mesh Collection to use for spline mesh segments.

Default: None

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false and Collection Source = Asset_

</details>

<details>

<summary><strong>Attribute Set Details</strong> <code>FPCGExRoamingAssetCollectionDetails</code></summary>

Configuration for building collection from attribute set data.

Default: Mesh collection type

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false and Collection Source = Attribute Set_

</details>

<details>

<summary><strong>Collection Path Attribute Name</strong> <code>FName</code></summary>

Attribute containing collection path for per-point collections.

Default: `CollectionPath`

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false and Collection Source = Path Attribute_

</details>

#### Distribution

<details>

<summary><strong>Distribution Settings</strong> <code>FPCGExAssetDistributionDetails</code></summary>

Controls how mesh entries are selected from the collection when not using staged points. Includes category filtering, distribution mode, and seed configuration.

Default: Weighted random

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false_

</details>

<details>

<summary><strong>Material Distribution Settings</strong> <code>FPCGExMicroCacheDistributionDetails</code></summary>

Controls how material variants are selected for mesh collections with material options.

Default: Weighted random

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false_

</details>

#### Tangents

<details>

<summary><strong>Tangents</strong> <code>FPCGExTangentsDetails</code></summary>

Configures how tangent vectors are computed or sourced for spline mesh segments. Options include:

* **Source**: Attribute-based or factory-generated tangents
* **Attribute Names**: Arrive/Leave tangent attribute names
* **Tangents Factory**: Reference to tangent generation factory
* **Scaling**: Tangent length scaling configuration

Default: Attribute-based tangents (`ArriveTangent`, `LeaveTangent`)

⚡ PCG Overridable

</details>

#### Fitting

<details>

<summary><strong>Scale To Fit</strong> <code>FPCGExScaleToFitDetails</code></summary>

Scale mesh segments to fit within bounds. Controls uniform or per-axis scaling strategies.

Default: None (no scaling)

📋 _Visible when Use Staged Points = false_

//→ See TODO FPCGExScaleToFitDetails

</details>

<details>

<summary><strong>Justification</strong> <code>FPCGExJustificationDetails</code></summary>

Position mesh segments within bounds using justification rules per axis.

Default: Centered justification

⚡ PCG Overridable

📋 _Visible when Use Staged Points = false_

</details>

<details>

<summary><strong>Read Translation</strong> <code>bool</code></summary>

Read fitting translation offset from staged points and apply to spline mesh segments.

**Translation Attribute Name** `FName`: Attribute containing translation offset.

Default: Disabled, attribute name `FittingOffset`

⚡ PCG Overridable (attribute name)

📋 _Visible when Use Staged Points = true_

</details>

#### Expansion

<details>

<summary><strong>Expansion</strong> <code>FPCGExSplineMeshMutationDetails</code></summary>

Push start/end points of spline mesh segments to create gaps or overlaps. Supports:

* **Push Start/End**: Enable pushing on either end
* **Amount**: Push distance (constant or attribute)
* **Relative**: Whether push is relative to segment length

Default: No expansion

⚡ PCG Overridable

</details>

#### Outputs

<details>

<summary><strong>Asset Path Attribute Name</strong> <code>FName</code></summary>

Attribute to write the mesh asset path to on each path point.

Default: `AssetPath`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tagging Details</strong> <code>FPCGExAssetTaggingDetails</code></summary>

Controls component tagging, including forwarding input data tags and grabbing tags from assets.

Default: Forward input tags, grab from asset

⚡ PCG Overridable

</details>

<details>

<summary><strong>Weight To Attribute</strong> <code>EPCGExWeightOutputMode</code></summary>

Write entry weight to an attribute with optional normalization.

**Weight Attribute Name** `FName`: Attribute to write weight to.

Default: No Output, attribute name `AssetWeight`

⚡ PCG Overridable

</details>

#### Spline Mesh Configuration

<details>

<summary><strong>Spline Mesh Up Mode</strong> <code>EPCGExSplineMeshUpMode</code></summary>

How the spline mesh up vector is determined.

| Option        | Description                               |
| ------------- | ----------------------------------------- |
| **Constant**  | Use a constant up vector for all segments |
| **Attribute** | Read up vector from a per-point attribute |
| **Tangents**  | Derive up vector from tangent data        |

Default: `Constant`

</details>

<details>

<summary><strong>Spline Mesh Up Vector</strong> <code>FVector</code></summary>

Constant up vector when Spline Mesh Up Mode is Constant.

**Spline Mesh Up Vector (Attr)** `FPCGAttributePropertyInputSelector`: Attribute selector when using Attribute mode.

Default: `(0, 0, 1)` (up)

⚡ PCG Overridable

</details>

#### Component Properties

<details>

<summary><strong>Default Descriptor</strong> <code>FPCGExStaticMeshComponentDescriptor</code></summary>

Default static mesh component properties applied to all spline mesh components. Includes rendering, collision, physics, shadows, LOD, and many other component settings.

**Force Default Descriptor** `bool`: Override collection descriptor settings with defaults.

Default: Standard descriptor settings

⚡ PCG Overridable (force flag)

</details>

<details>

<summary><strong>Property Override Descriptions</strong> <code>TArray</code></summary>

Additional property overrides to apply to spawned components using PCG's property override system.

Default: Empty

</details>

#### Target & Post-Processing

<details>

<summary><strong>Target Actor</strong> <code>TSoftObjectPtr</code></summary>

Actor to attach spline mesh components to. If not specified, components are attached to a generated actor.

Default: None (auto-generate)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Post Process Function Names</strong> <code>TArray</code></summary>

List of parameter-less functions to call on the target actor after spline mesh creation. Functions must have the "CallInEditor" flag enabled.

Default: Empty

</details>

#### Inherited Settings

This node inherits point filtering capabilities from its base class.

→ Point filters can be applied to control which paths are processed

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExStagingSplineMesh.h)
