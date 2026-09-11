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
| `BACKGROUND_COLOUR` | `USINT` | `1` | Attribute ID for the background colour index of a working set. |
| `SELECTABLE`        | `USINT` | `2` | Attribute ID indicating whether the working set can be selected by the operator (0 = FALSE, 1 = TRUE). |
| `ACTIVE_MASK`       | `USINT` | `3` | Attribute ID for the object ID of the data or alarm mask to be displayed when the working set is active. |

### **Adapters**

None.

## Functionality

The AID_WS constants are used to access and manipulate attributes of working set objects in an ISOBUS environment. Each constant corresponds to a specific attribute ID defined in the ISOBUS standard (Part 1 – Part 8, especially Part 6 for Virtual Terminal). By using these constants in conjunction with service functions or protocol requests, an application can query or set the background colour, selectable flag, or active mask of a working set.

The values are defined as `USINT` (unsigned short integer) and follow the AID numbering scheme from the ISOBUS standard. For example, the background colour attribute is assigned ID 1, the selectable attribute ID 2, and the active mask attribute ID 3. This mapping is consistent across all ISOBUS-compliant implementations.

## Technical Features

- **Global Availability**: The constants are defined as `VAR_GLOBAL CONSTANT`, meaning they are accessible from any function block or resource within the same application without explicit declaration.
- **Type Safety**: All constants are of type `USINT`, which matches the attribute ID data type used in ISOBUS protocol messages.
- **Documentation**: Each constant is accompanied by a comment explaining its meaning, derived from the ISOBUS standard.
- **Compatibility**: The constants align with the ISOBUS attribute IDs, ensuring interoperability with other ISOBUS-compliant devices.
- **No Dynamic Behavior**: As a global constants type, AID_WS does not contain any logic or state; it is purely a definition.

## State Overview

Since AID_WS is a constant definition, it has no runtime state. The values are fixed at compile time and cannot be modified during execution. This ensures that all references to these attributes are consistent throughout the application.

## Application Scenarios

AID_WS is typically used in ISOBUS Virtual Terminal (VT) implementations, particularly in function blocks that handle working set objects. Common use cases include:

- Creating or configuring a working set on a VT by assigning the background colour index.
- Determining whether a working set is selectable and acting accordingly.
- Setting the active mask (data or alarm) that should be displayed when the working set becomes active.

For example, a function block might use `AID_WS::BACKGROUND_COLOUR` in a request to set the background of a working set, or it might parse a response and use `AID_WS::SELECTABLE` to check the selectable status.

## Comparison with Similar Blocks

In ISOBUS systems, several other attribute ID sets exist for different object types, such as AID_OM (Object Masks), AID_AUX (Auxiliary Functions), etc. AID_WS specifically targets the working set object, while other constants sets would be used for other object types. Unlike function blocks that execute algorithms, AID_WS is a data definition, so it cannot be compared to functional blocks in terms of behavior. However, it serves a similar purpose to other global constant types: providing named, standardized values to avoid hardcoded numbers.

## Conclusion

AID_WS provides a clear and standardized set of constant definitions for working set attribute IDs in ISOBUS applications. By using these constants, developers can write more readable and maintainable code that is compliant with the ISOBUS standard. The absence of dynamic behavior ensures reliability and consistency. This global constants type is an essential building block for any IEC 61499 application that interacts with ISOBUS Virtual Terminal working sets.