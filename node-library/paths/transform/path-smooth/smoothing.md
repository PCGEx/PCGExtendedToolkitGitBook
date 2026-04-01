---
description: 'Base factory type for smoothing algorithms used by Path : Smooth.'
icon: arrow-down-from-arc
---

# Smoothing

### Overview

Smoothing methods define how neighboring points are gathered and how their influence weights are computed. The actual blending of properties and attributes is handled by the parent node's BlendOps configuration - the smoothing method only determines which neighbors participate and how heavily each one is weighted.

### Available Methods

| Method             | Description                                                                                                                                   |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **Moving Average** | Gathers neighbors within a sliding window along the path. Uses triangular weighting where closer neighbors in path order have more influence. |
| **Radius**         | Gathers all points within a spatial radius, ignoring path order. Uses inverse squared distance weighting.                                     |

### Choosing a Method

* **Moving Average**: Best when path topology matters. Only considers sequential neighbors along the path, so points that are spatially close but far apart in path order won't influence each other.
* **Radius**: Best when spatial proximity matters more than path order. Points influence each other based on 3D distance regardless of their position in the path sequence.

### How Smoothing Works

The smoothing method feeds into the BlendOps system:

1. **Gather Neighbors**: The method identifies which points should influence the target point.
2. **Compute Weights**: Each neighbor receives a weight based on the method's algorithm.
3. **Blend via BlendOps**: The parent node's BlendOps use these weights to blend properties/attributes from neighbors onto the target point.

This architecture means smoothing is attribute-agnostic - the same neighbor weights can smooth positions, colors, custom attributes, or any combination based on BlendOps configuration.

### Common Parameters

All smoothing methods receive these values from the parent Path : Smooth node:

| Parameter     | Description                                                                                                                |
| ------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Smoothing** | Controls the neighbor gathering range. For Moving Average, this is the window size. For Radius, this is the search radius. |
| **Influence** | How much to blend toward the smoothed result (0-1).                                                                        |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Smoothing/PCGExSmoothingInstancedFactory.h)
