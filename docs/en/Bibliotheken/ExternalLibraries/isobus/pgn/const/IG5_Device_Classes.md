# IG5_Device_Classes

![IG5_Device_Classes](./IG5_Device_Classes.svg)

* * * * * * * * * *
## Introduction

The **IG5_Device_Classes** global constants type defines device class identifiers for the Industry Group 5 (IG5) within the ISOBUS protocol. These constants are used to specify the type of vehicle system or equipment in a J1939 network, particularly for industrial-process control stationary applications (e.g., generator sets). The type is part of the `isobus.pgn.const` package and provides named values for the raw BYTE codes that appear in PGNs.

## Interface Structure

This type does not represent a typical function block with event or data inputs/outputs. Instead, it declares global constants that can be referenced throughout a 4diac application. The following subsections list the standard interface elements.

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

The **IG5_Device_Classes** type defines two constants:

| Constant Name         | Value (BYTE) | Description                                            |
|-----------------------|--------------|--------------------------------------------------------|
| `DC_INDUSTRIAL_GEN_SET` | `0`          | Device class for industrial-process control stationary (generator sets). |
| `DC_NOT_AVAILABLE`      | `127`        | Indicates that the device class is not available.      |

These constants are intended to be used in contexts where a device class needs to be identified in compliance with ISOBUS Industry Group 5 specifications. They help ensure consistent naming and avoid magic numbers in application logic.

## Technical Features

- **Type**: Defined as `BYTE` (8-bit unsigned integer), matching the J1939 data size.
- **Package**: `isobus.pgn.const` – grouped with other PGN-related constants.
- **Scope**: Global constants, accessible from any function block within the project.
- **Naming Convention**: Prefix `DC_` (Device Class) followed by a descriptive suffix.
- **Values**: Standardized according to ISOBUS definitions; `0` for generator sets, `127` for "not available".

## State Overview

Not applicable – this type does not define any state machines or behavioral states. It solely provides constant values.

## Application Scenarios

- **Generator Set Monitoring**: When building an application that reads or writes the device class from a J1939 message, use `DC_INDUSTRIAL_GEN_SET` to identify the device as a stationary generator set.
- **Fallback Handling**: Use `DC_NOT_AVAILABLE` to represent an unknown or uninitialized device class, facilitating error handling or default scenarios.
- **Protocol Compliance**: Referencing these constants ensures that your application adheres to the ISOBUS Industry Group 5 naming and value conventions, improving interoperability.

## Comparison with Similar Blocks

Since **IG5_Device_Classes** is a global constant type, it can be compared to other constant types that define device classes for different industry groups (e.g., IG1, IG2). Those would follow a similar pattern but with different value sets and package names. Unlike function blocks or adapters, this type has no runtime behavior; it only provides compile-time constants.

## Conclusion

The **IG5_Device_Classes** global constants type offers a clean, portable way to reference Industry Group 5 device class codes in 4diac applications. By using these named constants, developers can avoid hard-coding numeric values, increase code readability, and align with ISOBUS standards. Its simplicity and clear documentation make it a valuable asset for any project dealing with vehicle systems or industrial-process control equipment.