---
description: Retrieves a single GUID from a specific point index as param data.
icon: circle
---

# Get GUID

### Overview

This node generates a single GUID (Globally Unique Identifier) for a specific point in a collection using the same computation as the Write GUID node. Instead of writing GUIDs to all points as attributes, it extracts one GUID at a specified index and outputs it as param data. The GUID can be deterministic based on point properties (position, grid location, custom key) and output as an integer or string.

### How It Works

1. **Index Resolution**: Determines which point to read using Index parameter and safety mode
2. **GUID Computation**: Calculates GUID using the same logic as Write GUID
3. **Uniqueness Factors**: Combines selected factors (grid, position, key) for uniqueness
4. **Format Conversion**: Converts to integer or string based on output type
5. **Param Output**: Outputs single GUID value as param data

### Behavior

**Basic GUID Retrieval:**

```
Collection: 10 points
Index: 5

Reads point 5, computes GUID, outputs as param data
Result: Single GUID value (e.g., 1234567890 or "A1B2C3D4...")
```

**Index Safety Modes:**

```
Collection: 10 points (indices 0-9)
Index: 15 (out of range)

IndexSafety = Ignore:
  Error or undefined behavior

IndexSafety = Tile:
  Index = 15 % 10 = 5 (wraps around)
  → Reads point 5

IndexSafety = Clamp:
  Index = min(15, 9) = 9 (clamped to max)
  → Reads point 9

IndexSafety = Yoyo:
  Index bounces: 0,1,2...9,8,7...1,0,1,2...
  Index 15 → Point 5 (yoyo pattern)
```

**Output Types:**

```
OutputType = Integer:
  GUID → int64 value
  Example: 1234567890123456

OutputType = String:
  GUID → formatted string
  Format depends on Format setting
```

**String Formats:**

```
GUID: A1B2C3D4-E5F6-7890-ABCD-EF1234567890

Format = Digits:
  "A1B2C3D4E5F67890ABCDEF1234567890"

Format = DigitsLower:
  "a1b2c3d4e5f67890abcdef1234567890"

Format = DigitsWithHyphens:
  "A1B2C3D4-E5F6-7890-ABCD-EF1234567890"

Format = DigitsWithHyphensLower:
  "a1b2c3d4-e5f6-7890-abcd-ef1234567890"

Format = DigitsWithHyphensInBraces:
  "{A1B2C3D4-E5F6-7890-ABCD-EF1234567890}"
```

**Uniqueness Factors:**

```
Uniqueness flags (bitmask):
- Grid: Uses point's grid position
- Position: Uses point's world position (with collision handling)
- Key: Uses custom unique key value

Uniqueness = Grid + Position:
  GUID computed from grid location and position
  Same physical location → Same GUID

Uniqueness = Key only:
  GUID computed from unique key
  Different keys → Different GUIDs
```

**Hash Collision Handling:**

```
PositionHashCollision: (0.001, 0.001, 0.001)

Two points at nearly same position:
Point A: (100.0000, 200.0000, 0.0)
Point B: (100.0005, 200.0005, 0.0)

Difference < collision threshold:
  Both hash to same grid cell → Same GUID component
  (Prevents floating-point precision issues)
```

Good for: lookup keys, deterministic IDs, single-point identification, param data generation

### Settings

<details>

<summary><strong>Index</strong> <code>int32</code></summary>

Point index to retrieve GUID from.

The GUID will be computed for the point at this index in the collection.

Default: `0`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle out-of-range indices.

| Mode                 | Description                        |
| -------------------- | ---------------------------------- |
| **Ignore** (default) | Out of range causes error          |
| **Tile**             | Wrap around (modulo)               |
| **Clamp**            | Clamp to valid range \[0, count-1] |
| **Yoyo**             | Bounce back and forth              |

Default: `Ignore`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Config</strong> <code>FPCGExGUIDDetails</code></summary>

GUID generation configuration.

Controls output attribute name, type, format, and uniqueness factors.

⚡ PCG Overridable

</details>

#### GUID Configuration (FPCGExGUIDDetails)

<details>

<summary><strong>Output Attribute Name</strong> <code>FName</code></summary>

Name of the output attribute in the param data.

Default: `"GUID"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Type</strong> <code>EPCGExGUIDOutputType</code></summary>

Data type for the GUID output.

| Type                  | Description                |
| --------------------- | -------------------------- |
| **Integer** (default) | Output as int64            |
| **String**            | Output as formatted string |

**Integer**: Compact numeric representation **String**: Human-readable, configurable format

Default: `Integer`

</details>

<details>

<summary><strong>Format</strong> <code>EPCGExGUIDFormat</code></summary>

String format when Output Type = String.

| Format                        | Example                                |
| ----------------------------- | -------------------------------------- |
| **Digits** (default)          | A1B2C3D4E5F67890ABCDEF1234567890       |
| **DigitsLower**               | a1b2c3d4e5f67890abcdef1234567890       |
| **DigitsWithHyphens**         | A1B2C3D4-E5F6-7890-ABCD-EF1234567890   |
| **DigitsWithHyphensLower**    | a1b2c3d4-e5f6-7890-abcd-ef1234567890   |
| **DigitsWithHyphensInBraces** | {A1B2C3D4-E5F6-7890-ABCD-EF1234567890} |

Default: `Digits`

</details>

<details>

<summary><strong>Uniqueness</strong> <code>uint8 (bitmask)</code></summary>

Bitmask flags determining which factors contribute to GUID uniqueness.

Flags (combined with bitwise OR):

* **Grid**: Grid cell location
* **Position**: World space position
* **Key**: Custom unique key value
* **All**: All factors (default)

Different combinations produce different levels of uniqueness.

Default: `All`

</details>

<details>

<summary><strong>Unique Key Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the unique key value.

* **Constant** (default): Use fixed Unique Key value
* **Attribute**: Read from point attribute

⚡ PCG Overridable

</details>

<details>

<summary><strong>Unique Key</strong> <code>int32</code></summary>

Custom key value for GUID uniqueness.

Used when Unique Key Input = Constant or as fallback.

Default: `42`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grid Hash Collision</strong> <code>FVector</code></summary>

Grid cell size for spatial hashing.

Points within this distance hash to the same grid cell, preventing floating-point precision issues.

Default: `(0.001, 0.001, 0.001)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Position Hash Collision</strong> <code>FVector</code></summary>

Position tolerance for hash collision handling.

Positions within this threshold are considered the same for hashing.

Default: `(0.001, 0.001, 0.001)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Position Hash Offset</strong> <code>FVector</code></summary>

Offset applied to position before hashing.

Allows shifting the spatial hash grid.

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Interpolation</strong> <code>bool</code></summary>

Whether to allow interpolation in param data.

Technical setting for param data behavior.

Default: `true`

⚡ PCG Overridable

</details>

### Practical Examples

**Get GUID for First Point:**

```
Index: 0
Use as lookup key or seed for that specific point
```

**Deterministic Seeding:**

```
Use GUID (integer) as random seed
Same point position → Same GUID → Reproducible randomness
```

**Lookup Key Generation:**

```
Extract GUID for central point
Use as database key or reference ID
```

### Inputs

| Pin    | Type       | Description                   |
| ------ | ---------- | ----------------------------- |
| **In** | Point Data | Point collection to read from |

### Outputs

| Pin      | Type       | Description                           |
| -------- | ---------- | ------------------------------------- |
| **GUID** | Param Data | Single GUID value (integer or string) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Utils/PCGExGetGUID.h)
