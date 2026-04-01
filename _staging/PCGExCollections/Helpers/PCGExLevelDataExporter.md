---
icon: cog
description: 'Level Data Exporter - Abstract base class for level-to-PCGDataAsset conversion'
---

# Level Data Exporter

Abstract base class for level-to-PCGDataAsset conversion.

## Overview

Level Data Exporter defines the interface for converting a loaded UWorld into a `UPCGDataAsset`. It is instanced directly on Level Collection entries via Unreal's `EditInlineNew` pattern, so subclasses can expose custom properties in the collection's details panel. The base class itself has no settings — all behavior comes from subclasses.

## How It Works

1. **Instancing**: A Level Collection entry references a Level Data Exporter instance. The exporter is created inline and configured per-entry.
2. **Export Call**: When staging data is rebuilt, the collection calls `ExportLevelData` with the loaded world and a target `UPCGDataAsset`. The asset's `TaggedData` is cleared before the call.
3. **Custom Logic**: Subclasses populate the data asset with point data, metadata, and optionally embedded collections.

#### Usage Notes

- **Abstract**: This class cannot be used directly. Use the built-in Default Level Data Exporter, or subclass in C++ or Blueprint.
- **Blueprintable**: The `ExportLevelData` function is a `BlueprintNativeEvent`, so Blueprint subclasses can override the export logic without C++.
- **Inline Editing**: Because the class uses `EditInlineNew` and `DefaultToInstanced`, derived exporters appear as editable sub-objects directly inside the Level Collection entry's details panel.

## Settings

No settings. This is an abstract base class. See subclass documentation for configuration.

### Subclasses

- **Default Level Data Exporter** — Classifies actors as Mesh or Actor, creates typed point data, and optionally generates embedded collections.

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Helpers/PCGExLevelDataExporter.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 0 (abstract base class with no UPROPERTYs)
Inherited Properties: None (derives from UObject)
Inputs: N/A (utility class)
Outputs: N/A (utility class)
Nested Types: None
-->
