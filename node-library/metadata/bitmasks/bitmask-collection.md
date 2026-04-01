---
description: >-
  Data asset that stores named bitmask entries for reusable bitmask
  configurations.
icon: book
---

# Bitmask Collection

### Overview

A Bitmask Collection is a data asset that serves as a library of named bitmask configurations, each optionally paired with a direction vector. Collections enable reusable bitmask definitions that can be referenced across multiple nodes, providing centralized management of bitmask patterns used for filtering, adjacency checking, or categorical flagging. The caching system ensures efficient lookup when collections are referenced repeatedly.

### How It Works

1. **Asset Creation**: Create a Bitmask Collection data asset in the Content Browser
2. **Entry Definition**: Add named entries, each with an identifier, bitmask, and optional direction
3. **Cache Building**: Collection builds an internal cache for fast name-to-bitmask lookups
4. **Node Reference**: Nodes that support bitmask collections can reference this asset
5. **Identifier Lookup**: Nodes query the collection by identifier name to retrieve bitmasks
6. **Direction Usage**: Optional direction vectors enable directional adjacency or orientation-based operations

**Usage Notes**

* **Reusability**: Collections allow defining bitmasks once and using them across multiple nodes and graphs.
* **Centralized Management**: Changing a bitmask in the collection updates it everywhere it's referenced.
* **Identifier Uniqueness**: Each entry's identifier should be unique within the collection for unambiguous lookups.
* **Direction Normalization**: Direction vectors are automatically normalized when retrieved, so magnitude doesn't matter.
* **Caching**: The collection caches bitmask data for performance - modifications trigger cache rebuilds.

### Collection Structure

#### FPCGExBitmaskCollectionEntry

Each entry in the collection contains:

**Identifier** (`FName`):

* Unique name to reference this bitmask entry
* Used for lookups when nodes query the collection
* Default: `None`

**Bitmask** (`FPCGExBitmask`):

* The bitmask value for this entry
* Contains flag settings and values

**Direction** (`FVector`):

* Optional direction vector associated with this entry
* Used for directional adjacency or orientation-based operations
* Automatically normalized when retrieved
* Default: `(0, 0, 1)` (Up vector)

### Behavior

```
Bitmask Collection Asset:

Entry 1:
  Identifier: "Cardinal"
  Bitmask: [Flags for N, S, E, W adjacency]
  Direction: (0, 0, 1)

Entry 2:
  Identifier: "Diagonal"
  Bitmask: [Flags for NE, SE, SW, NW adjacency]
  Direction: (1, 0, 0)

Entry 3:
  Identifier: "All"
  Bitmask: [All adjacency flags]
  Direction: (0, 0, 0)
```

**Lookup by Identifier:**

```
Collection referenced by node:

Node queries "Cardinal" → Returns bitmask + direction (0, 0, 1)
Node queries "Diagonal" → Returns bitmask + direction (1, 0, 0)
Node queries "Unknown" → Returns false (not found)
```

**Direction Usage Example:**

```
Directional Adjacency Check:

Entry "North":
  Bitmask: [North flag = 1]
  Direction: (0, 1, 0)

Entry "South":
  Bitmask: [South flag = 1]
  Direction: (0, -1, 0)

Node can check adjacency AND get the direction vector
for orientation-based operations.
```

**Shared Collections:**

```
Multiple nodes reference same collection:

[Bitmask Collection Asset]
  ├─ Filter Node A (uses "Cardinal")
  ├─ Filter Node B (uses "Diagonal")
  └─ Adjacency Node (uses "All")

Changing "Cardinal" in collection updates all references.
```

### Creating a Collection

1. **Create Asset**:
   * Right-click in Content Browser
   * Create → Data Asset → PCGEx Bitmask Collection
2. **Add Entries**:
   * Add new entries to the Entries array
   * Set unique Identifier for each
   * Configure Bitmask flags/values
   * Optionally set Direction vector
3. **Use in Nodes**:
   * Reference the collection asset in compatible nodes
   * Query by identifier name to retrieve bitmasks

### Settings

<details>

<summary><strong>Notes</strong> <code>FString</code></summary>

Developer notes and comments about this collection.

Editor-only data for documentation purposes.

</details>

<details>

<summary><strong>Entries</strong> <code>TArray</code></summary>

Array of named bitmask entries in this collection.

Each entry consists of:

* **Identifier**: Unique name for lookup
* **Bitmask**: The bitmask configuration
* **Direction**: Optional direction vector

Displayed with format: `{Identifier} | {Direction}`

</details>

### Practical Examples

**Adjacency Library:**

```
Collection: "GridAdjacency"
  - "Cardinal": N, S, E, W flags, Direction: Up
  - "Diagonal": NE, SE, SW, NW flags, Direction: Right
  - "All8": All 8 directions, Direction: Zero
```

**Categorical Flags:**

```
Collection: "TerrainTypes"
  - "Walkable": Ground, Path, Road flags
  - "Water": Ocean, River, Lake flags
  - "Blocked": Wall, Cliff, Obstacle flags
```

**Directional Filters:**

```
Collection: "WindDirections"
  - "North": North flag, Direction: (0, 1, 0)
  - "East": East flag, Direction: (1, 0, 0)
  - "South": South flag, Direction: (0, -1, 0)
  - "West": West flag, Direction: (-1, 0, 0)
```

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Data/Bitmasks/PCGExBitmaskCollection.h)
