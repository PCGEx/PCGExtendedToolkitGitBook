---
description: Writes cluster states as an int64 flag mask attribute on vertices.
icon: circle-nodes
---

# Cluster : Write States

### Overview

This node evaluates connected state providers against each vertex in the cluster and writes the combined result as an int64 flag attribute. State providers can perform various checks (filters, adjacency patterns, etc.) and contribute bits to the final flag value. This enables efficient state-based filtering and selection in downstream nodes using bitmask operations.

### How It Works

1. **State Initialization**: Creates a flag attribute initialized with the specified initial value.
2. **State Evaluation**: For each vertex, evaluates all connected state providers.
3. **Flag Composition**: Each state provider applies its bitmask operations (AND, OR, XOR, etc.) to the flag value.
4. **Attribute Write**: Writes the final composed flag value to each vertex.

**Usage Notes**

* **State Providers**: Connect state provider sub-nodes to the States input pin to define what flags are written.
* **Bitmask Operations**: State providers can set, clear, or toggle specific bits based on their evaluation results.
* **Initial Flags**: Use to pre-set certain bits before state evaluation begins.
* **Downstream Use**: The flag attribute can be used with bitmask filters for efficient vertex selection.

### Behavior

```
State Providers:                Output Flag Attribute:

[State A] → sets bit 0          Vertex 1: 0b00000011 (states A,B passed)
[State B] → sets bit 1          Vertex 2: 0b00000001 (only state A passed)
[State C] → sets bit 2          Vertex 3: 0b00000111 (all states passed)
```

### Inputs

| Pin        | Type            | Description                                               |
| ---------- | --------------- | --------------------------------------------------------- |
| **Vtx**    | Points          | Cluster vertices                                          |
| **Edges**  | Points          | Cluster edges                                             |
| **States** | State Factories | State providers that evaluate conditions and modify flags |

### Settings

<details>

<summary><strong>Flag Attribute</strong> <code>FName</code></summary>

The name of the int64 attribute to write the computed flags to.

Default: `Flags`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Initial Flags</strong> <code>int64</code></summary>

The initial flag value before any state providers modify it. Use to pre-set certain bits.

Default: `0`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type   | Description                          |
| --------- | ------ | ------------------------------------ |
| **Vtx**   | Points | Cluster vertices with flag attribute |
| **Edges** | Points | Cluster edges (pass-through)         |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/States/PCGExClusterWriteStates.h)
