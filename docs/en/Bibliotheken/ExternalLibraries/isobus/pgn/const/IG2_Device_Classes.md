# IG2_Device_Classes

![IG2_Device_Classes](./IG2_Device_Classes.svg)

* * * * * * * * * *
## Introduction

The `IG2_Device_Classes` type defines a set of global constants for **Industry Group 2** (vehicle systems) as specified by the ISOBUS standard (ISO 11783). These constants provide standardized BYTE values that identify the device class of agricultural equipment, such as tractors, harvesters, and sprayers. They are intended for use in 4diac applications that communicate via ISOBUS and need to assign or decode device class information in PGN (Parameter Group Number) messages.

## Interface Structure

This type is a **global constant definition**, not a function block, adapter, or subapplication. Consequently, it possesses no event or data interfaces. The constants themselves are the entire content of this type.

### **Event Inputs**

*Not applicable* — no event inputs are defined.

### **Event Outputs**

*Not applicable* — no event outputs are defined.

### **Data Inputs**

*Not applicable* — no data inputs are defined.

### **Data Outputs**

*Not applicable* — no data outputs are defined.

### **Adapters**

*Not applicable* — no adapter interfaces are defined.

## Functionality

The primary purpose of `IG2_Device_Classes` is to supply a consistent, human‑readable set of constants that map to the device class codes used in ISOBUS. Each constant is declared as a `BYTE` (8‑bit unsigned integer) with a fixed value. These constants are typically referenced in FB code (e.g., in ST or Structured Text) to set or compare the device class field of a message, ensuring that all parts of an application use the same numeric representation.

The following table lists all constants defined in this type:

| Constant Name | Value (BYTE) | Description |
|--------------|--------------|-------------|
| `DC_NON_SPECIFIC_SYSTEM` | 0 | Non‑specific system |
| `DC_TRACTOR` | 1 | Tractor |
| `DC_TILLAGE` | 2 | Tillage |
| `DC_SECONDARY_TILLAGE` | 3 | Secondary tillage |
| `DC_PLANTERS_SEEDERS` | 4 | Planters / Seeders |
| `DC_FERTILIZERS` | 5 | Fertilizers |
| `DC_SPRAYERS` | 6 | Sprayers |
| `DC_HARVESTERS` | 7 | Harvesters |
| `DC_ROOT_HARVESTERS` | 8 | Root harvesters |
| `DC_FORAGE` | 9 | Forage |
| `DC_IRRIGATION` | 10 | Irrigation |
| `DC_TRANSPORT_TRAILER` | 11 | Transport / Trailer |
| `DC_FARM_YARD_OPERATIONS` | 12 | Farm yard operations |
| `DC_POWERED_AUXILIARY_DEVICES` | 13 | Powered auxiliary devices |
| `DC_SPECIAL_CROPS` | 14 | Special crops |
| `DC_EARTH_WORK` | 15 | Earth work |
| `DC_SKIDDER` | 16 | Skidder |
| `DC_SENSOR_SYSTEMS` | 17 | Sensor systems |
| `DC_TIMBER_HARVESTERS` | 19 | Timber harvesters |
| `DC_FORWARDERS` | 20 | Forwarders |
| `DC_TIMBER_LOADERS` | 21 | Timber loaders |
| `DC_TIMBER_PROCESSING_MACHINES` | 22 | Timber processing machines |
| `DC_MULCHERS` | 23 | Mulchers |
| `DC_UTILITY_VEHICLES` | 24 | Utility vehicles |
| `DC_SLURRY_MANURE_APPLICATORS` | 25 | Slurry / Manure applicators |
| `DC_FEEDERS_MIXERS` | 26 | Feeders / Mixers |
| `DC_WEEDERS` | 27 | Weeders – non‑chemical weed control |
| `DC_TURF_AND_LAWN_CARE_MOWERS` | 28 | Turf and lawn care mowers |
| `DC_PRODUCT_MATERIAL_HANDLING` | 29 | Product / Material handling |
| `DC_NOT_AVAILABLE` | 127 | Not available |

All values are declared as `CONSTANT` and are therefore immutable at runtime. Using these constants instead of raw numeric literals improves code readability and maintainability, especially when dealing with ISOBUS messages that carry device class information.

## Technical Features

- **Data type:** `BYTE` (unsigned 8‑bit integer)
- **Scope:** Global — accessible from any FB or subapplication in the project
- **Predefined values:** 31 distinct constants covering the standard device classes defined in ISOBUS Industry Group 2
- **Immutability:** Constants are declared with the `CONSTANT` keyword and cannot be changed during execution
- **Standard compliance:** Values match the assignments defined by the ISOBUS standard (ISO 11783)

## State Overview

This type is not a stateful entity. It does not contain any state variables, internal algorithms, or event‑driven behavior. The constants are static and exist at compile time, providing a fixed mapping between names and numeric codes.

## Application Scenarios

Typical use cases for `IG2_Device_Classes` include:

- **Message composition:** When building a PGN that includes a device‑class field, assign a constant such as `DC_TRACTOR` to the corresponding byte.
- **Message parsing:** When decoding an incoming PGN, compare the received device‑class value with constants to determine the type of equipment.
- **Configuration:** Use the constants in parameter files or configuration dialogs to let users select a device class from a list of human‑readable names.
- **Filtering:** In fleet management or diagnostics, filter messages based on the device class to process only relevant data.

By using these global constants, applications avoid magic numbers and stay aligned with the ISOBUS standard, reducing errors and easing code review.

## Comparison with Similar Blocks

In the 4diac‑IDE environment, `IG2_Device_Classes` is a **global constant type** rather than a function block or adapter. The differences are:

- **Function blocks** contain executable logic and have inputs/outputs, whereas this type only provides named constants.
- **Adapters** define communication interfaces between subapplications; this type has no such capability.
- Compared to a simple variable declaration, using a global constant type centralizes all device‑class definitions in one place, simplifying maintenance and ensuring consistency across a project.

## Conclusion

The `IG2_Device_Classes` global constants offer a standardized, type‑safe method for referencing ISOBUS device classes in 4diac applications. They are invaluable for developers working with agricultural machinery communication, as they eliminate arbitrary numeric values and provide self‑documenting code. The constants are easy to include in any FB or subapplication and directly support the requirements of the ISOBUS protocol, making them a fundamental building block for compliant systems.