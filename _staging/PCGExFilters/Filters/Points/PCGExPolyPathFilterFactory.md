---
icon: puzzle-piece
description: 'Poly Path Filter Factory - Abstract base for path/spline inclusion filters'
---

# Poly Path Filter Factory

Abstract base for path/spline inclusion filters.

## Overview

Poly Path Filter Factory is the abstract base class behind spatial inclusion filters that test points against paths, splines, and polygons. It handles loading target data, converting it into projected polyline representations, building a spatial octree for fast lookups, and providing the inclusion testing infrastructure that concrete filter subclasses use.

This class is not used directly. Concrete filters like **Filter : Inclusion (Path/Splines)** inherit from it and expose user-facing settings.

## How It Works

1. **Load Targets**: Reads path, spline, or polygon data from the target input pin
2. **Build PolyPaths**: Converts each target into a projected polyline representation, respecting fidelity, expansion, and winding settings
3. **Octree Construction**: Builds a spatial octree from the expanded bounds of each polypath for fast spatial queries
4. **Handler Creation**: Concrete filters create a Handler that evaluates points against the polypath set using inside/outside/on logic

#### Usage Notes

- **Abstract**: This class cannot be instantiated directly. Use one of its concrete subclasses.
- **Multi-Format Input**: Accepts point data (paths), spline data, and polygon 2D data (5.7+) as targets
- **Projection-Based**: Inclusion testing works in 2D projected space. The projection settings determine how 3D positions map to the testing plane.
- **Self-Exclusion**: By default, a path does not test against itself when the input and target sets overlap
- **Match Rules**: Optional match rule sub-nodes can filter which targets are considered for each input collection

## Settings

No user-facing settings. Configuration is exposed through concrete subclasses.

### Subclasses

- **Filter : Inclusion (Path/Splines)** — Tests whether points are inside, outside, or on path/spline boundaries

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExPolyPathFilterFactory.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 0 (abstract base, all config through protected members set by subclasses)
Inherited Properties: Referenced to UPCGExPointFilterFactoryData
Inputs: Targets (spatial data), Match Rules (factories) - declared by subclasses
Outputs: Filter (Factory) - inherited
Nested Types: EPCGExSplineSamplingIncludeMode, EPCGExSplineCheckType, EPCGExSplineFilterPick (enums used by subclasses)
-->
