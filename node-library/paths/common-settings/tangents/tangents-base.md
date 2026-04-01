---
description: Base factory and configuration structures for tangent calculation systems.
icon: arrow-down-from-arc
---

# Tangents (Base)

### Overview

This is the foundational framework for all tangent calculation factories used in PCGEx. It defines the common interface, configuration structures, and tangent source modes that control how arrive and leave tangents are determined for path and spline operations. All specific tangent factories (Auto, Catmull-Rom, From Neighbors, From Transform, Zero) inherit from this base.

### Tangent Sources

Tangents can come from three different sources:

| Source        | Description                                | Use Case                                    |
| ------------- | ------------------------------------------ | ------------------------------------------- |
| **None**      | No tangents calculated or used             | Disable tangent processing                  |
| **Attribute** | Read tangent vectors from point attributes | Pre-computed tangents stored as data        |
| **In-place**  | Calculate tangents using a factory module  | Dynamic tangent generation using algorithms |

### Tangent Factories

When using **In-place** mode, choose from these tangent calculation methods:

* **Auto** - Geometric tangents from neighboring point positions
* **Catmull-Rom** - Classic spline interpolation (C1 continuous)
* **From Neighbors** - Direction-averaged bisector tangents
* **From Transform** - Transform-aligned tangents based on point rotation
* **Zero** - Zero-length tangents (linear interpolation)

### Configuration Structures

#### FPCGExTangentsDetails

Complete tangent configuration including source, attributes, factories, and scaling.

**Source** (`EPCGExTangentSource`):

* Controls where tangent data comes from (None, Attribute, In-place)

**Arrive Tangent Attribute** (`FName`):

* Attribute name to read arrive tangents from when Source = Attribute
* Default: `"ArriveTangent"`

**Leave Tangent Attribute** (`FName`):

* Attribute name to read leave tangents from when Source = Attribute
* Default: `"LeaveTangent"`

**Tangents** (`UPCGExTangentsInstancedFactory`):

* Factory used to calculate tangents in-place
* Visible when Source = In-place

**Start Override** (`UPCGExTangentsInstancedFactory`):

* Optional factory specifically for the first point
* Visible when Source = In-place

**End Override** (`UPCGExTangentsInstancedFactory`):

* Optional factory specifically for the last point
* Visible when Source = In-place

**Scaling** (`FPCGExTangentsScalingDetails`):

* Scale factors for arrive and leave tangents (see below)

#### FPCGExTangentsScalingDetails

Controls tangent length through scale multipliers.

**Arrive Scale Input** (`EPCGExInputValueType`):

* Source for arrive tangent scale: Constant or Attribute

**Arrive Scale (Constant)** (`double`):

* Scale multiplier for arrive tangents when using constant
* Default: `1.0`
* ⚡ PCG Overridable

**Arrive Scale (Attribute)** (`FPCGAttributePropertyInputSelector`):

* Attribute to read arrive scale from when using attribute input
* ⚡ PCG Overridable

**Leave Scale Input** (`EPCGExInputValueType`):

* Source for leave tangent scale: Constant or Attribute

**Leave Scale (Constant)** (`double`):

* Scale multiplier for leave tangents when using constant
* Default: `1.0`
* ⚡ PCG Overridable

**Leave Scale (Attribute)** (`FPCGAttributePropertyInputSelector`):

* Attribute to read leave scale from when using attribute input
* ⚡ PCG Overridable

### How Tangents Work

**Arrive vs Leave Tangents:**

```
For a point on a path:

  ← Arrive tangent   [Point]   Leave tangent →

Arrive: Tangent vector "arriving at" the point (influences incoming curve)
Leave: Tangent vector "leaving" the point (influences outgoing curve)
```

**Edge Point Handling:**

```
Open Path (3 points):

  [Start] ──→ [Middle] ──→ [End]

Start point:
  - Arrive tangent: Special handling (StartTangents override or default)
  - Leave tangent: Toward middle point

Middle point:
  - Arrive tangent: From previous point
  - Leave tangent: Toward next point

End point:
  - Arrive tangent: From previous point
  - Leave tangent: Special handling (EndTangents override or default)
```

**Start/End Overrides:**

```
Tangents: Auto (for all points)
Start Override: From Transform (for first point only)
End Override: Zero (for last point only)

Result:
  First point → Uses From Transform
  Middle points → Use Auto
  Last point → Uses Zero
```

**Scaling:**

```
Base tangent direction: (1, 0, 0)

ArriveScale = 1.0, LeaveScale = 1.0:
  Arrive: (1, 0, 0)
  Leave: (1, 0, 0)

ArriveScale = 2.0, LeaveScale = 0.5:
  Arrive: (2, 0, 0)  (longer)
  Leave: (0.5, 0, 0) (shorter)

Scale controls tangent length, affecting curve smoothness/tightness.
```

### Usage Notes

**Factory Selection**: Different tangent factories suit different use cases - neighbor-based methods for geometric curves, transform-based for oriented paths, Catmull-Rom for classic splines.

**Edge Point Control**: Use Start/End Overrides to give first and last points different tangent behavior than middle points.

**Scaling Strategy**: Arrive and leave scales can be set independently, allowing asymmetric curve influence at each point.

**Attribute Mode**: When tangents are pre-computed and stored as attributes, use Source = Attribute to read them directly without recalculation.

### Related Implementations

This base class is implemented by:

* Auto Tangents
* Catmull-Rom Tangents
* From Neighbors Tangents
* From Transform Tangents
* Zero Tangents

Used by nodes such as:

* Write Tangents
* Path operations requiring curve smoothness

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Tangents/PCGExTangentsInstancedFactory.h)
