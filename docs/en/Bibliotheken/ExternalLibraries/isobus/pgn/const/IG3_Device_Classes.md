# IG3_Device_Classes

![IG3_Device_Classes](./IG3_Device_Classes.svg)

* * * * * * * * * *
## Introduction

The `IG3_Device_Classes` global constant definition provides a standardized set of byte‑encoded identifiers for Industry Group 3 (IG3) vehicle systems, specifically those used in construction, agricultural, and mobile machinery applications. These constants align with the SAE J1939 / ISOBUS 11783 protocol family and are intended to be used within PGN (Parameter Group Number) communications to clearly identify the type of system or device class being addressed. By using these symbolic names instead of raw numeric values, source code becomes more readable, maintainable, and less error‑prone.

## Interface Structure

This element is **not** a function block, adapter, or subapplication. It is a global constant set, which means it has no event inputs, event outputs, data inputs, data outputs, or adapter connections. Instead, it exposes a collection of named constants of type `BYTE` that can be referenced throughout an automation project.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The constants represent the officially assigned device classes for Industry Group 3 as defined by the ISOBUS standard. Each constant holds a distinct integer value (`0` to `17` and `127`) that corresponds to a specific vehicle system or a “not available” state. The values are used in fields of PGN messages to indicate the type of equipment on the network, facilitating correct data interpretation and routing between ECUs.

## Technical Features

- **Data Type:** All constants are of type `BYTE`, ranging from 0 to 255.
- **Naming Convention:** Each constant is prefixed with `DC_` (Device Class) and follows a descriptive uppercase name.
- **Value Assignment:** Values are fixed and conform to the ISOBUS specification:
  - `0` – Non‑specific system
  - `1` – Skid Steer Loader
  - `2` – Articulated Dump Truck
  - `3` – Backhoe
  - `4` – Crawler
  - `5` – Excavator
  - `6` – Forklift
  - `7` – Four Wheel Drive Loader
  - `8` – Grader
  - `9` – Milling Machine
  - `10` – Recycler and Soil Stabilizer
  - `11` – Binding Agent Spreader
  - `12` – Paver
  - `13` – Feeder
  - `14` – Screening Plant
  - `15` – Stacker
  - `16` – Roller
  - `17` – Crusher
  - `127` – Not Available (used when the class is unknown or not specified)
- **Compiler Integration:** The constant set is declared with a compiler package name `isobus::pgn::const`, indicating that it is part of a C++‑like namespace for ISOBUS PGN constants, enabling seamless inclusion in embedded firmware projects.

## State Overview

Not applicable – the element defines compile‑time constants; no runtime state exists.

## Application Scenarios

- **ISOBUS Network Communication:** When constructing or parsing PGNs (e.g., the “Vehicle System” fields), developers can directly use `DC_EXCAVATOR` instead of the numeric value `5`.
- **Device Identification:** In diagnostic or management messages, the constant helps identify the type of attached implement or vehicle, enabling appropriate control and display.
- **Firmware Development:** Embedded software for ECUs can include this constant set to ensure consistent device class reporting across different hardware variants.

## Comparison with Similar Blocks

Unlike function blocks that contain algorithms and state machines, this global constant set has no executable logic. It is comparable to header files or enumerations in conventional programming. Other constant sets may exist for different industry groups (e.g., IG1, IG2) or for other PGN parameters, but this specific set focuses solely on IG3 device classes. Using symbolic constants avoids magic numbers and improves code clarity and maintainability.

## Conclusion

The `IG3_Device_Classes` constant set is a foundational element for any project that uses ISOBUS (ISO 11783) communication in the construction and agricultural machinery domain. By providing readable, standardized names for device class values, it promotes code consistency, reduces mistakes, and simplifies integration. It is an essential resource for developers working on vehicle control units, implement controllers, or diagnostic tools that interact with IG3 systems.