---
description: A simple texture parameter definition.
icon: circle-dashed
---

# Texture Param

### Overview

This sub-node defines parameters for sampling textures from materials. It specifies which material texture parameter to sample, what channels to read, and how to output the sampled values. Multiple Texture Param nodes can be used together to sample different textures or channels from the same material.

### How It Works

1. **Identify Texture**: Locates the texture using the material parameter name.
2. **Configure Channels**: Specifies which color channels (R, G, B, A) to sample.
3. **Set Output Type**: Determines the attribute type for sampled values.
4. **Apply Scale**: Optionally scales the output values.

**Usage Notes**

* **Material Parameter**: The parameter name must match a texture parameter in the material.
* **Channel Selection**: Sample specific channels or combinations (RGB, RGBA).
* **Output Types**: Auto-detect based on channels, or force specific types (Vector4, Vector, etc.).
* **Texture Arrays**: Supports sampling from texture arrays using an index.

### Settings

#### Parameter Identification

<details>

<summary><strong>Material Parameter Name</strong> <code>FName</code></summary>

Name of the texture parameter to look for in the material.

Default: `TextureParameter`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Texture ID Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to output the texture identifier to.

Default: `TextureId`

⚡ PCG Overridable

</details>

#### Sampling

<details>

<summary><strong>Sample Attribute Name</strong> <code>FName</code></summary>

Name of the attribute to output the sampled value to.

Default: `Sample`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Type</strong> <code>EPCGExTexSampleAttributeType</code></summary>

Type of the attribute to output the sampled value to.

| Option      | Description                             |
| ----------- | --------------------------------------- |
| **Auto**    | Output type driven by selected channels |
| **Double**  | Output as Double                        |
| **Vector4** | Output as Vector4 (RGBA)                |
| **Vector**  | Output as Vector (RGB)                  |
| **Vector2** | Output as Vector2 (RG)                  |

Default: `Auto`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sampled Channels</strong> <code>EPCGExTexChannelsFlags</code></summary>

Which color channels to sample from the texture.

| Flag     | Description                |
| -------- | -------------------------- |
| **R**    | Red channel                |
| **G**    | Green channel              |
| **B**    | Blue channel               |
| **A**    | Alpha channel              |
| **RGB**  | RGB channels (omits alpha) |
| **RGBA** | All channels               |

Default: `RGBA`

</details>

<details>

<summary><strong>Scale</strong> <code>double</code></summary>

Scale factor applied to the output value.

Default: `1`

⚡ PCG Overridable

</details>

#### Texture Array

<details>

<summary><strong>Texture Index Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the texture array index.

| Option        | Description                     |
| ------------- | ------------------------------- |
| **Constant**  | Use fixed index value           |
| **Attribute** | Read index from point attribute |

Default: `Constant`

</details>

<details>

<summary><strong>Texture Index (Attr)</strong> <code>FName</code></summary>

Attribute containing the texture array index.

Default: `TextureIndex`

📋 _Visible when Texture Index Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Texture Index</strong> <code>int32</code></summary>

Fixed texture array index. Use -1 for non-array textures.

Default: `-1`

📋 _Visible when Texture Index Input = Constant_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSampling-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSampling/Public/Core/PCGExTexParamFactoryProvider.h)
