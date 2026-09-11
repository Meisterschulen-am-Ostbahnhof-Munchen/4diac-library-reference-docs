# IG1_Device_Classes

![IG1_Device_Classes](./IG1_Device_Classes.svg)

* * * * * * * * * *
## Introduction
The `IG1_Device_Classes` is a global constants definition used in the 4diac IDE environment, specifically designed for Industry Group 1 (IG1) of ISO 11783 (ISOBUS) applications. It provides a set of predefined, application‑independent device class identifiers that represent different types of vehicle systems within the agricultural and forestry domain. These constants allow consistent referencing of device classes throughout a project, enhancing code readability and maintainability.

## Interface Structure
This element is not a function block (FB) or adapter, but rather a global constants container. Consequently, it does not expose event inputs, event outputs, data inputs, data outputs, or adapters in the conventional sense. The information it provides is a set of constant values that can be used globally within the application.

### **Event Inputs**
None.

### **Event Outputs**
None.

### **Data Inputs**
None.

### **Data Outputs**
None (the constants are accessible as global read‑only values, not as classical block outputs).

### **Adapters**
None.

## Functionality
The global constants defined here correspond to the device class byte values used in ISOBUS Parameter Group Numbers (PGNs) to indicate the type of vehicle system. They cover the most common categories as specified by the Industry Group 1 standard:

| Constant Name | Value (BYTE) | Description |
|---------------|--------------|-------------|
| `DC_NON_SPECIFIC_SYSTEM` | 0 | Device Class / Vehicle System: Non‑specific System |
| `DC_TRACTOR` | 1 | Device Class / Vehicle System: Tractor |
| `DC_TRAILER` | 2 | Device Class / Vehicle System: Trailer |
| `DC_NOT_AVAILABLE` | 127 | Device Class / Vehicle System: Not Available |

These constants are intended to be used wherever a device class needs to be compared or set, ensuring that the same numeric values are always referenced by meaningful symbolic names.

## Technical Features
- **Data Type:** All constants are of type `BYTE` (8‑bit unsigned integer), matching the ISOBUS device class encoding.
- **Initial Values:** Each constant is assigned a fixed initial value that cannot be changed at runtime.
- **Global Scope:** The constants are declared as `VAR_GLOBAL CONSTANT`, making them accessible from any function block or sub‑application within the project.
- **Standard Compliance:** The values follow the ISOBUS 11783 standard, specifically the Industry Group 1 device class assignments.
- **Namespace Organisation:** The associated `CompilerInfo` indicates that the constants belong to the `isobus::pgn::const` package, providing a clear logical grouping.

## State Overview
Not applicable – this element does not contain any internal state or runtime behaviour. It is a static definition.

## Application Scenarios
- **ISOBUS Communication:** When implementing ISOBUS communication stacks or interacting with electronic control units (ECUs), the device class is often embedded in a PGN. These constants can be used to set or decode the device class field.
- **Fleet Management:** In applications that differentiate between tractors, trailers, or other implements, these constants allow clear, error‑free classification.
- **Configuration Tools:** Software tools that configure vehicle systems can utilise these constants to present user‑friendly names instead of raw numeric values.
- **Testing and Simulation:** During simulation of ISOBUS messages, the constants help generate realistic data without hard‑coding magic numbers.

## Comparison with Similar Blocks
Other global constant definitions exist for other Industry Groups (e.g., `IG0_Device_Classes`, `IG2_Device_Classes`), each containing a different set of device classes. In contrast, this set is specific to Industry Group 1, focusing on tractors, trailers, and non‑specific systems. There are also byte‑enumeration type definitions (e.g., enum data types in ST) that could serve a similar purpose, but global constants are simpler and require no additional data types. When compared to hard‑coded literals, using these constants significantly improves code maintainability and reduces mistakes.

## Conclusion
`IG1_Device_Classes` provides a compact and standardised set of global constants for representing ISOBUS Industry Group 1 device classes. Even though it is not a function block, it plays a vital role in the development of ISOBUS‑based applications by promoting code readability, consistency, and compliance with international standards. Its simplicity and global availability make it an indispensable building block for any project involving tractor, trailer, or other non‑specific agricultural vehicle systems.