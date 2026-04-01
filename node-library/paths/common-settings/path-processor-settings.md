---
description: Base infrastructure for path processing operations.
icon: code-commit
---

# Path Processor Settings

### Overview

This is the foundational processor settings class for operations that work with paths in PCGExtendedToolkit. A path is an ordered sequence of points representing a line, spline, route, or trajectory. This base class provides common infrastructure for all path-based operations, handling path validation, closed loop support, and output filtering for invalid paths.

### How It Works

1. **Path Input**: Accepts point collections representing paths (ordered point sequences)
2. **Validation**: Checks if paths have the minimum required points (typically 2 or more)
3. **Processing**: Derived classes perform specific path operations (smoothing, sampling, analysis, etc.)
4. **Output Filtering**: Optionally omits invalid paths (less than 2 points) from output
5. **Closed Loop Support**: Can handle paths that form closed loops (first point connects to last)

Derived classes implement specific path operations like smoothing, resampling, subdivision, tangent calculation, or path analysis.

### Common Settings

All path processor types inherit these base settings:

<details>

<summary><strong>Omit Invalid Paths Outputs</strong> <code>bool</code></summary>

Controls whether paths with fewer than 2 points are omitted from the output or passed through.

* **false**: Invalid paths (less than 2 points) are passed through unchanged
* **true** (default): Invalid paths are omitted from the output entirely

A path needs at least 2 points to represent a meaningful line or trajectory. Single-point or empty paths are considered invalid.

**When to disable**: If you need to preserve all input data regardless of validity, or if downstream nodes need to process or track invalid paths.

**When to enable** (default): For cleaner output and to avoid processing errors in nodes that expect valid paths.

Default: `true`

</details>

### Closed Loop Support

Many path processors support closed loops, where the path forms a continuous circuit by connecting the last point back to the first. This is useful for:

* Circular paths or orbits
* Closed boundaries or perimeters
* Looping trajectories
* Ring-shaped structures

Check individual processor documentation to see specific closed loop behavior.

### Path Definition

A **valid path** consists of:

* **Minimum**: 2 points (defines a line segment)
* **Typical**: 3+ points (defines a polyline or curve)
* **Order matters**: Points define the path sequence
* **Closed option**: Last point can connect back to first

An **invalid path** has:

* 0 points (empty)
* 1 point (no direction or length)

### Usage Pattern

Path processors are used for:

1. **Path Smoothing**: Smoothing sharp corners or noise
2. **Resampling**: Changing point density along paths
3. **Subdivision**: Adding intermediate points
4. **Analysis**: Computing tangents, lengths, curvature
5. **Transformation**: Modifying path shape or properties
6. **Extraction**: Deriving data from path geometry

Each derived processor specializes in a specific path operation type.

### Inputs

| Pin       | Type       | Description                                    |
| --------- | ---------- | ---------------------------------------------- |
| **Paths** | Point Data | Ordered sequences of points representing paths |

### Outputs

| Pin       | Type       | Description                    |
| --------- | ---------- | ------------------------------ |
| **Paths** | Point Data | Processed path point sequences |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Core/PCGExPathProcessor.h)
