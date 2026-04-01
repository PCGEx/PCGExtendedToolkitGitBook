---
description: Generates dummy data entries to drive loop iterations.
icon: circle
---

# Iterations

### Overview

This node creates multiple duplicate entries of a lightweight dummy data object to serve as iteration drivers for loop nodes. Instead of providing actual data, it outputs N identical data entries that PCG loop nodes can iterate over. This is useful for controlling loop counts when you need a specific number of iterations without depending on input data.

### How It Works

1. **Type Selection**: Determines the PCG data type to output (Params, Points, Spline, etc.)
2. **Data Creation**: Creates a single lightweight dummy data object
3. **Duplication**: Adds N duplicate references to the same object
4. **Utils Generation** (optional): Adds per-iteration index/progress attributes
5. **Output**: Produces N data entries for loop consumption

**Usage Notes**

* **Loop Control**: This node is primarily used to drive loop nodes when you need a specific number of iterations regardless of input data.
* **Lightweight**: The dummy data is minimal - all iterations reference the same underlying object to minimize memory.
* **Subgraph Compatibility**: Using typed outputs (Points, Spline, etc.) allows subgraphs to work both as loops and as regular processing graphs with properly typed pins.
* **No Input Required**: This node doesn't require input data - it generates iteration drivers independently.

### Behavior

**Basic Iteration Generation:**

```
Iterations: 5
Type: Attribute Set (Params)

Output: 5 identical param data entries
→ Loop node processes 5 times
```

**Loop Integration:**

```
[Iterations] → Iterations: 10
  ↓
[Loop] → Processes 10 times
  ↓ Each iteration
[Your Processing]
```

**Without Iterations Node:**

```
[Data with 3 collections]
  ↓
[Loop] → Processes 3 times (once per collection)
```

**With Iterations Node Override:**

```
[Iterations] → Iterations: 100
  ↓ (overrides data count)
[Loop] → Processes 100 times
```

**Output Utils:**

```
bOutputUtils = true
Iterations: 5

Each iteration entry gets attributes:
- Index: 0, 1, 2, 3, 4 (current iteration)
- Progress: 0.0, 0.25, 0.5, 0.75, 1.0 (normalized 0-1)

Useful for progress-driven operations within loop
```

**Data Types:**

```
Type = Attribute Set (Params):
  Outputs param data (lightweight)
  Most efficient for pure iteration

Type = Points:
  Outputs point data (single point per entry)
  Compatible with point-processing loops

Type = Spline:
  Outputs spline data
  For spline-aware loop contexts

Type = Texture:
  Outputs texture data
  For texture processing loops

Type = Any:
  Untyped pin (flexible connection)
```

Good for: loop count control, fixed iterations, batch processing, repetition control

### Settings

<details>

<summary><strong>Type</strong> <code>EPCGExIterationDataType</code></summary>

Type of dummy data to generate.

| Type                        | Description                            |
| --------------------------- | -------------------------------------- |
| **Any**                     | Untyped pin (attribute set internally) |
| **Attribute Set** (default) | Param data (most efficient)            |
| **Points**                  | Point data (single point per entry)    |
| **Spline**                  | Spline data                            |
| **Texture**                 | Texture data                           |

Choosing the right type ensures pin compatibility with downstream loop nodes.

**Attribute Set**: Best for pure iteration control (lightweight) **Points**: When loop expects point data **Spline/Texture**: For specialized loop contexts

Default: `Attribute Set`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Iterations</strong> <code>int32</code></summary>

Number of iteration entries to generate.

This determines how many times a connected loop will execute.

Examples:

* `10`: Loop processes 10 times
* `100`: Loop processes 100 times
* `0`: No iterations (loop skipped)

Default: `0`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Utils</strong> <code>bool</code></summary>

Output per-iteration param data with useful index and progress attributes.

When enabled, each iteration entry contains:

* **Index**: Current iteration number (0, 1, 2, ...)
* **Progress**: Normalized progress (0.0 to 1.0)

Less optimized than the basic version but provides iteration context.

📋 _Visible when Type = Attribute Set_

Default: `false`

⚡ PCG Overridable

</details>

### Practical Examples

**Fixed Iteration Count:**

```
[Iterations: 20]
  ↓
[Loop]
  ↓
[Spawn Random Points] → Executes 20 times
```

**Batch Processing:**

```
[Iterations: 50]
  ↓
[Loop]
  ↓
[Heavy Operation] → Splits work into 50 batches
```

**Progress-Driven Effects:**

```
[Iterations: 100, OutputUtils: true]
  ↓
[Loop]
  ↓
[Use Progress Attribute] → Fade/ramp effect over 100 steps
```

**Override Data-Driven Loops:**

```
[Data: 3 collections] ──→ [Loop] (would process 3×)
[Iterations: 10] ────────↗ (overrides to 10×)
```

### Inputs

| Pin    | Type | Description        |
| ------ | ---- | ------------------ |
| (none) | -    | No inputs required |

### Outputs

| Pin            | Type           | Description                                     |
| -------------- | -------------- | ----------------------------------------------- |
| **Iterations** | Varies by Type | N duplicate dummy data entries for loop driving |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Utils/PCGExIterations.h)
