# AID_WS

![AID_WS](./AID_WS.svg)

* * * * * * * * * *

## Introduction

AID_WS is a set of global constants used in the ISOBUS (ISO 11783) working set object attribute identifiers. These constants define the attribute IDs (AIDs) for working set objects, which are essential for identifying and manipulating properties of a working set within an ISOBUS VT (Virtual Terminal) system. The constants provide a standardized way to reference background colour, selectable state, and the active mask of a working set, ensuring consistent and reliable communication between the VT and the implement control unit (ECU).

This global constants type is part of the `isobus::UT::Q::const::AID` package and is intended to be used in applications that implement ISOBUS-based user interface components. By using these predefined constants, developers can avoid magic numbers and improve code readability and maintainability.

## Interface Structure

AID_WS is a global constants definition and does not have any event inputs, event outputs, data inputs, data outputs, or adapters. However, it exposes a set of constant values that can be referenced globally within the IEC 61499 application. These constants are listed below as equivalent to data outputs for clarity.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following constant values are defined by AID_WS and are accessible globally:

| Constant Name | Type   | Value | Description |
|---------------|--------|-------|-------------|
| `BACKGROUND_COLOUR` | `USINT` | `[1]` | Attribute ID for background colour index. **Read-only** for *Change Attribute* (ISO 11783-6 Table B.1). Supplied during object pool creation; queryable via *Get Attribute Value* (F.58). |
| `SELECTABLE`        | `USINT` | `[2]` | Attribute ID for selectable status (0 = FALSE, 1 = TRUE). **Read-only** for *Change Attribute* (ISO 11783-6 Table B.1). Supplied during object pool creation; queryable via *Get Attribute Value* (F.58). |
| `ACTIVE_MASK`       | `USINT` | `[3]` | Attribute ID for active data or alarm mask object ID. **Read-only** for *Change Attribute* (ISO 11783-6 Table B.1). Queryable via *Get Attribute Value* (F.58); updated at runtime via *Change Active Mask* (F.34). |

### **Adapters**

None.

## Functionality

The AID_WS constants provide numeric attribute identifiers for working set objects in an ISOBUS environment (ISO 11783-6 Table B.1).

All three attributes (`BACKGROUND_COLOUR`, `SELECTABLE`, and `ACTIVE_MASK`) are **read-only** for the *Change Attribute* command (F.38). At runtime, these attribute IDs are queried via *Get Attribute Value* (F.58). To change the active mask at runtime, applications must use the specialized *Change Active Mask* command (F.34) rather than *Change Attribute*.

The values are defined as `USINT` (unsigned short integer) and follow the AID numbering scheme from ISO 11783-6 Table B.1.

## Technical Features

- **Global Availability**: The constants are defined as `VAR_GLOBAL CONSTANT`, meaning they are accessible from any function block or resource within the same application without explicit declaration.
- **Type Safety**: All constants are of type `USINT`, matching the attribute ID data type used in ISOBUS protocol messages.
- **Standards Compliance**: All three Working Set attributes are read-only for *Change Attribute* (F.38) per ISO 11783-6 Table B.1.
- **No Dynamic Behavior**: As a global constants type, AID_WS does not contain any logic or state; it is purely a definition.

## State Overview

Since AID_WS is a constant definition, it has no runtime state. The values are fixed at compile time and cannot be modified during execution. This ensures that all references to these attributes are consistent throughout the application.

## Application Scenarios

AID_WS is typically used in ISOBUS Virtual Terminal (VT) implementations, particularly in function blocks that handle working set objects. Common use cases include:

- Defining working set attributes (background colour, selectable flag, initial active mask) during object pool construction.
- Querying working set attributes at runtime via *Get Attribute Value* (F.58).
- Switching the active data or alarm mask at runtime using the specialized *Change Active Mask* command (F.34).

For example, a function block might use `AID_WS::BACKGROUND_COLOUR` in a request to set the background of a working set, or it might parse a response and use `AID_WS::SELECTABLE` to check the selectable status.

## Comparison with Similar Blocks

In ISOBUS systems, several other attribute ID sets exist for different object types, such as AID_OM (Object Masks), AID_AUX (Auxiliary Functions), etc. AID_WS specifically targets the working set object, while other constants sets would be used for other object types. Unlike function blocks that execute algorithms, AID_WS is a data definition, so it cannot be compared to functional blocks in terms of behavior. However, it serves a similar purpose to other global constant types: providing named, standardized values to avoid hardcoded numbers.

## Conclusion

AID_WS provides a clear and standardized set of constant definitions for working set attribute IDs in ISOBUS applications. By using these constants, developers can write more readable and maintainable code that is compliant with the ISOBUS standard. The absence of dynamic behavior ensures reliability and consistency. This global constants type is an essential building block for any IEC 61499 application that interacts with ISOBUS Virtual Terminal working sets.
