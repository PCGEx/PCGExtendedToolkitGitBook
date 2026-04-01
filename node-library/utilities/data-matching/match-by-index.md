---
description: Matches elements based on index positions.
icon: circle-dashed
---

# Match : By Index

### Overview

This match rule establishes correspondences between data elements based on their index positions or index attribute values. It can either compare the target's index attribute against the candidate's position index, or vice versa, providing flexible index-based matching for ordered data. The rule includes safeguards for handling out-of-range indices through various wrapping and clamping strategies.

### How It Works

1. **Source Selection**: Determines which dataset provides the index value:
   * **Target**: Reads index attribute from target, compares to candidate's position
   * **Candidate**: Reads index attribute from candidate, compares to target's position
2. **Index Attribute Read**: Retrieves the index value from the specified IndexAttribute
3. **Index Comparison**: Compares the read index value against the position index
4. **Safety Handling**: Applies IndexSafety strategy for out-of-bounds indices:
   * **Tile**: Wraps indices using modulo (repeating pattern)
   * **Clamp**: Clamps indices to valid range
   * **Ignore**: Treats out-of-range as no match
5. **Match Evaluation**: Elements match if their indices correspond after safety handling
6. **Strictness Application**: Applies inherited Strictness setting
7. **Inversion**: Applies inherited Invert flag if enabled

### Behavior

Source Mode Examples:

**Target Mode:**

```
Target[0] has IndexAttr=5 → Matches Candidate[5]
Target[1] has IndexAttr=2 → Matches Candidate[2]
Target[2] has IndexAttr=0 → Matches Candidate[0]
```

**Candidate Mode:**

```
Candidate[0] has IndexAttr=3 → Matches Target[3]
Candidate[1] has IndexAttr=1 → Matches Target[1]
Candidate[2] has IndexAttr=4 → Matches Target[4]
```

**Index Safety Examples (Candidate has 5 elements \[0-4]):**

```
Index=7 with Tile:   7 % 5 = 2 → Matches Candidate[2]
Index=7 with Clamp:  min(7, 4) = 4 → Matches Candidate[4]
Index=7 with Ignore: Out of range → No Match

Index=-2 with Tile:   ((-2 % 5) + 5) % 5 = 3 → Matches Candidate[3]
Index=-2 with Clamp:  max(-2, 0) = 0 → Matches Candidate[0]
Index=-2 with Ignore: Out of range → No Match

Default Attribute ($Index):
If using the default $Index attribute, this becomes a
simple position-to-position match (element 0 → 0, 1 → 1, etc.)
```

Good for: sequential pairing, ordered correspondences, array-like matching, indexed lookups, synchronized datasets

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Source</strong> <code>EPCGExMatchByIndexSource</code></summary>

Determines which dataset provides the index value to use for matching.

| Option               | Description                                                                                                                                                    |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Target** (default) | Reads the index attribute value from the target element and compares it against the position index of the candidate. Target controls which candidate to match. |
| **Candidate**        | Reads the index attribute value from the candidate element and compares it against the position index of the target. Candidate controls which target to match. |

Example:

* **Target**: Target\[i].IndexAttr → Candidate\[IndexAttr]
* **Candidate**: Candidate\[i].IndexAttr → Target\[IndexAttr]

Default: `Target`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read for the index value. Only supports @Data domain and will only attempt to read from there.

* **"$Index"** (default): Uses the built-in element index
* **"CustomIndex"**: Reads a custom index attribute
* **"LookupID"**: Reads a lookup ID attribute

The attribute must contain integer values representing indices.

Default: `"$Index"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

Determines how to handle index values that fall outside the valid range (0 to element count - 1).

| Option             | Description                                                                                              |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| **Ignore**         | Out-of-range indices are treated as no match. Safe but strict.                                           |
| **Clamp**          | Clamps indices to valid range (negative → 0, too large → max). Matches boundary elements.                |
| **Tile** (default) | Wraps indices using modulo operation, creating repeating/cyclic pattern. Negative indices wrap from end. |
| **Yoyo**           | Bounces indices back and forth at boundaries (0,1,2,3,2,1,0,1,2...).                                     |

**Tile Example**: Index 7 in array of 5 → 7 % 5 = 2 **Clamp Example**: Index 7 in array of 5 → min(7, 4) = 4 **Yoyo Example**: Index 7 in array of 5 → bounces to 1

Default: `Tile`

⚡ PCG Overridable

</details>

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

**Tip**: Using the default $Index attribute with Source=Target and IndexSafety=Ignore creates simple position-to-position matching (element 0 to element 0, 1 to 1, etc.), which is useful for aligning ordered datasets.

### Outputs

| Pin            | Type       | Description                                 |
| -------------- | ---------- | ------------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory for index-based matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchByIndex.h)
