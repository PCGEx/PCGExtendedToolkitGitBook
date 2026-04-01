---
description: Writes unique GUIDs as attributes to all points in a collection.
icon: circle
---

# Write GUID

### Overview

This node generates Globally Unique Identifiers (GUIDs) for every point in a collection and writes them as point attributes. The GUIDs can be deterministic based on configurable uniqueness factors (point index, position, seed, grid location) and output as integers or strings in various formats. This enables reliable point identification, lookup keys, or deterministic seeding while maintaining consistency across regenerations.

### How It Works

1. **Attribute Preparation**: Creates the output attribute with the specified name and type
2. **Uniqueness Configuration**: Determines which factors contribute to GUID calculation (bitmask of flags)
3. **GUID Generation**: For each point, combines selected uniqueness factors into a GUID
4. **Format Conversion**: Converts GUID to integer or formatted string based on settings
5. **Attribute Writing**: Writes the GUID value to the point's attribute
6. **Output**: Produces point collection with GUID attribute on every point

**Usage Notes**

* **Determinism**: GUIDs are deterministic - same inputs (position, seed, index, etc.) produce the same GUID every time.
* **Integer vs String**: Integer GUIDs are more compact and faster to compare; String GUIDs are human-readable and easier to debug.
* **Uniqueness Factors**: Carefully choose which factors to include. Fewer factors = less unique but more reproducible; more factors = more unique but harder to recreate.
* **Hash Collisions**: The collision threshold settings prevent floating-point precision issues from creating different GUIDs for "nearly identical" positions.

### Behavior

**Basic GUID Writing:**

```
Input: 100 points
Config.OutputAttributeName: "GUID"
Config.OutputType: Integer

Output: 100 points, each with "GUID" attribute (int64)
Each point gets a unique identifier value.
```

**Uniqueness Factors (Bitmask):**

```
Uniqueness flags (combine with bitwise OR):
- Index: Point's index in collection
- Position: Point's world position (with collision handling)
- Seed: Point's seed value
- Grid: PCG Grid component location
- All: All factors combined (default)

Uniqueness = Position + Seed:
  GUID computed from position and seed only
  Same position + seed → Same GUID (reproducible)

Uniqueness = Index only:
  GUID computed from index
  Point 0 → GUID_A, Point 1 → GUID_B (sequential)
  Re-running with same order → Same GUIDs
```

**Output Types:**

```
OutputType = Integer:
  GUID → int64 value
  Example: 1234567890123456
  Compact, suitable for numeric operations

OutputType = String:
  GUID → formatted string
  Format depends on Format setting
  Human-readable, suitable for display/logging
```

**String Formats (10 Formats):**

```
GUID: BD048CE3-358B-46C5-8CEE-627C719418F8

Format = Digits:
  "BD048CE3358B46C58CEE627C719418F8"

Format = DigitsLower:
  "bd048ce3358b46c58cee627c719418f8"

Format = DigitsWithHyphens:
  "BD048CE3-358B-46C5-8CEE-627C719418F8"

Format = DigitsWithHyphensLower (RFC 4122):
  "bd048ce3-358b-46c5-8cee-627c719418f8"

Format = DigitsWithHyphensInBraces:
  "{BD048CE3-358B-46C5-8CEE-627C719418F8}"

Format = DigitsWithHyphensInParentheses:
  "(BD048CE3-358B-46C5-8CEE-627C719418F8)"

Format = HexValuesInBraces:
  "{0xBD048CE3,0x358B,0x46C5,{0x8C,0xEE,0x62,0x7C,0x71,0x94,0x18,0xF8}}"

Format = UniqueObjectGuid:
  "BD048CE3-358B46C5-8CEE627C-719418F8"

Format = Short (Base64):
  "vQSM4zWLRsWM7mJ8cZQY-A"

Format = Base36Encoded:
  "1DPF6ARFCM4XH5RMWPU8TGR0J"
```

**Hash Collision Handling:**

```
GridHashCollision: (0.001, 0.001, 0.001)
PositionHashCollision: (0.001, 0.001, 0.001)

Point A: Position (100.0000, 200.0000, 0.0)
Point B: Position (100.0005, 200.0005, 0.0)

Positions within collision threshold:
  Both hash to same grid cell
  → Treated as "same position" for GUID purposes
  → Prevents floating-point precision issues
```

**Position Hash Offset:**

```
PositionHashOffset: (10, 0, 0)

Point at (100, 200, 0):
  Hashed as (110, 200, 0)
  → Shifts the spatial hash grid

Useful for:
- Avoiding hash conflicts with other systems
- Creating variant GUIDs for same positions
```

**Unique Key Input:**

```
UniqueKeyInput = Constant:
  UniqueKeyConstant: 42
  All points use constant value 42 in GUID computation

UniqueKeyInput = Attribute:
  UniqueKeyAttribute: "CustomSeed"
  Each point uses its "CustomSeed" attribute value
  → Different seed values → Different GUIDs
```

**Deterministic Behavior:**

```
Two runs with same settings and data:
Run 1: Point at (100, 200, 0), Seed 5 → GUID = X
Run 2: Point at (100, 200, 0), Seed 5 → GUID = X (identical)

GUID generation is deterministic and reproducible
when uniqueness factors remain constant.
```

**Allow Interpolation:**

```
bAllowInterpolation = true:
  GUID attribute marked as interpolatable
  PCG may interpolate GUID values during sampling
  (Not recommended for GUIDs, but available)

bAllowInterpolation = false (recommended):
  GUID attribute non-interpolatable
  Prevents averaging/blending of identifiers
```

Good for: point identification, lookup keys, deterministic seeding, tracking, reference IDs

### Settings

<details>

<summary><strong>Config</strong> <code>FPCGExGUIDDetails</code></summary>

GUID generation configuration.

Controls output attribute name, type, format, and uniqueness factors.

All nested properties shown below.

⚡ PCG Overridable

</details>

#### GUID Configuration (FPCGExGUIDDetails)

<details>

<summary><strong>Output Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to write GUID values to.

This attribute will be created on all points with the GUID data type (int64 or FString depending on Output Type).

Default: `"GUID"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Type</strong> <code>EPCGExGUIDOutputType</code></summary>

Data type for the GUID output attribute.

| Type                  | Description                                      |
| --------------------- | ------------------------------------------------ |
| **Integer** (default) | Output as int64 (compact numeric representation) |
| **String**            | Output as formatted string (human-readable)      |

**Integer**: Best for numeric operations, comparisons, hashing **String**: Best for logging, display, debugging

Default: `Integer`

</details>

<details>

<summary><strong>Format</strong> <code>EPCGExGUIDFormat</code></summary>

String format when Output Type = String.

| Format                 | Example                                                               | Notes                                      |
| ---------------------- | --------------------------------------------------------------------- | ------------------------------------------ |
| **Digits** (default)   | BD048CE3358B46C58CEE627C719418F8                                      | 32 hex digits                              |
| **Digits (Lowercase)** | bd048ce3358b46c58cee627c719418f8                                      | 32 hex digits, lowercase                   |
| **Digits (Hyphens)**   | BD048CE3-358B-46C5-8CEE-627C719418F8                                  | Standard format with hyphens               |
| **Digits (RFC 4122)**  | bd048ce3-358b-46c5-8cee-627c719418f8                                  | RFC 4122 standard (lowercase with hyphens) |
| **{Digits}**           | {BD048CE3-358B-46C5-8CEE-627C719418F8}                                | Wrapped in braces                          |
| **(Digits)**           | (BD048CE3-358B-46C5-8CEE-627C719418F8)                                | Wrapped in parentheses                     |
| **{Hex}**              | {0xBD048CE3,0x358B,0x46C5,{0x8C,0xEE,0x62,0x7C,0x71,0x94,0x18,0xF8\}} | Comma-separated hex values                 |
| **Unique Object GUID** | BD048CE3-358B46C5-8CEE627C-719418F8                                   | Unreal FUniqueObjectGuid format            |
| **Short (Base64)**     | vQSM4zWLRsWM7mJ8cZQY-A                                                | Base64 with URL-safe characters            |
| **Short (Base36)**     | 1DPF6ARFCM4XH5RMWPU8TGR0J                                             | Base-36 (case-insensitive)                 |

Note: Format also affects Integer output, as the integer is the TypeHash of the formatted GUID string.

Default: `Digits`

</details>

<details>

<summary><strong>Uniqueness</strong> <code>uint8 (bitmask)</code></summary>

Bitmask flags determining which factors contribute to GUID uniqueness.

Flags (combined with bitwise OR):

* **Index**: Point's index in the collection
* **Position**: Point's world space position (with collision handling)
* **Seed**: Point's seed value
* **Grid**: PCG Grid component location
* **All**: All factors (default)

Different combinations produce different levels of uniqueness.

Examples:

* `Position + Seed`: Same position and seed → Same GUID
* `Index only`: Sequential GUIDs based on point order
* `All`: Maximum uniqueness (index + position + seed + grid)

Default: `All`

</details>

<details>

<summary><strong>Unique Key Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the unique key value used in GUID computation.

| Option                 | Description                               |
| ---------------------- | ----------------------------------------- |
| **Constant** (default) | Use fixed Unique Key value for all points |
| **Attribute**          | Read unique key from point attribute      |

**Constant**: Same base key for all points (uniformity) **Attribute**: Per-point key from attribute (per-point variation)

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Unique Key (Constant)</strong> <code>int32</code></summary>

Base value for GUID uniqueness when Unique Key Input = Constant.

Acts like a seed value mixed into GUID computation.

Examples:

* `42` (default): Standard base value
* `0`: Minimal base value
* `12345`: Custom seed

📋 _Visible when Unique Key Input = Constant_

Default: `42`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Unique Key (Attribute)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read unique key value from when Unique Key Input = Attribute.

Each point's attribute value will be used as its unique key contribution.

📋 _Visible when Unique Key Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Grid Hash Collision</strong> <code>FVector</code></summary>

Grid cell size for spatial hashing of grid location.

PCG Grid positions within this distance hash to the same grid cell.

Default: `(0.001, 0.001, 0.001)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Position Hash Collision</strong> <code>FVector</code></summary>

Position tolerance for hash collision handling.

Positions within this threshold are considered identical for hashing, preventing floating-point precision issues.

Default: `(0.001, 0.001, 0.001)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Position Hash Offset</strong> <code>FVector</code></summary>

Offset applied to position before hashing.

Shifts the spatial hash grid, useful for creating variant GUIDs or avoiding conflicts.

Default: `(0, 0, 0)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Interpolation</strong> <code>bool</code></summary>

Whether the GUID attribute allows interpolation during PCG sampling.

Generally should be false for GUIDs to prevent averaging/blending of identifiers.

Default: `true`

⚡ PCG Overridable

</details>

### Practical Examples

**Point Identification:**

```
[Point Collection]
  ↓
[Write GUID] OutputAttributeName: "ID", OutputType: Integer
  ↓
[Points with unique "ID" attribute for lookup/tracking]
```

**Deterministic Seeding:**

```
[Point Collection]
  ↓
[Write GUID] Uniqueness: Position + Seed
  ↓
[Use "GUID" as random seed for reproducible randomness per point]
```

**Variant Generation:**

```
[Same Point Collection]
  ↓
[Write GUID] UniqueKeyConstant: 100 → "GUID_A"
[Write GUID] UniqueKeyConstant: 200 → "GUID_B"
  ↓
[Two different GUID sets for same points]
```

### Inputs

| Pin    | Type       | Description                         |
| ------ | ---------- | ----------------------------------- |
| **In** | Point Data | Point collections to write GUIDs to |

### Outputs

| Pin     | Type       | Description                                         |
| ------- | ---------- | --------------------------------------------------- |
| **Out** | Point Data | Point collections with GUID attribute on all points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Utils/PCGExWriteGUID.h)
