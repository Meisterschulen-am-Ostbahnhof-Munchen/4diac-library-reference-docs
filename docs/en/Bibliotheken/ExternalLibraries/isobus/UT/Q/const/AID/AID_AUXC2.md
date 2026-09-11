# AID_AUXC2

![AID_AUXC2](./AID_AUXC2.svg)

* * * * * * * * * *

## Introduction

The AID_AUXC2 global constants provide the attribute identifiers for the Auxiliary Control Designator Type 2 (AuxC2) object within the ISOBUS (ISO 11783) protocol. These constants define the numeric IDs used to reference the pointer type and object ID attributes of an auxiliary control designator of type 2. The constants belong to the package `isobus::UT::Q::const::AID` and are used to standardize attribute access in ISOBUS device implementations.

## Interface Structure

AID_AUXC2 is a global constant container and does not expose a runtime interface. Consequently, it has no event or data inputs/outputs or adapters.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None. The following global constants are provided instead:

| Constant | Data Type | Initial Value | Description |
|----------|-----------|---------------|-------------|
| `PTR_TYPE` | USINT | `USINT#1` | Attribute ID 1 (`AID_AUXC2_PTR_TYPE`) – Pointer Type. Defines what the pointer of the AuxC2 object references: 0 = Points to Aux Object, 1 = Points to Aux Function/Input, 2 = Points to Working Set owner of pointer, 3 = Points to Working Set owner of assigned function/input. |
| `OBJ_ID`   | USINT | `USINT#2` | Attribute ID 2 (`AID_AUXC2_OBJ_ID`) – Object ID. Contains the object ID of a referenced Auxiliary Function or Auxiliary Input object, or NULL. |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The AID_AUXC2 constants are used as attribute identifiers when accessing the properties of an Auxiliary Control Designator Type 2 object in an ISOBUS system. In the ISOBUS standard, every object property is addressed via an attribute ID. These constants map the human-readable property names (`PTR_TYPE`, `OBJ_ID`) to their numeric attribute identifiers (1 and 2).

By using these pre-defined constants, application code can avoid magic numbers and ensure compliance with the ISOBUS attribute ID scheme. The `PTR_TYPE` constant distinguishes between different pointer destinations, while `OBJ_ID` specifies the referenced auxiliary object.

## Technical Features

- **Global constant container**: The constants are declared as `GLOBALCONSTANTS` and can be referenced throughout a 4diac-IDE project without instantiation.
- **Unsigned short integer type**: Both constants are of type `USINT` (8-bit unsigned integer) which matches the ISOBUS attribute ID field size.
- **Pre-initialized values**: The initial values correspond to the official ISOBUS attribute IDs.
- **Package scoping**: The constants are located in the package `isobus::UT::Q::const::AID`, providing a clear namespace for auxiliary control designator constants.
- **SPDX-License-Identifier**: The implementation is provided under the Eclipse Public License 2.0.

## State Overview

Since AID_AUXC2 is a compile-time constant set, it does not maintain any runtime state. No state transitions or lifecycle management is required.

## Application Scenarios

The AID_AUXC2 constants are intended to be used in ISOBUS-compliant agricultural machinery control systems that implement the Auxiliary Control Designator Type 2 functionality. Typical applications include:

- ISOBUS Universal Terminal (UT) implementations that need to read or write the auxiliary control designator properties.
- Implement control ECU (Tractor-Implement Management) software that processes AuxC2 objects.
- Diagnostic and configuration tools that operate on the auxiliary control interface.

## Comparison with Similar Blocks

The AID_AUXC2 constants are specifically tailored for Auxiliary Control Designator Type 2 objects. Other constant sets exist in the `isobus::UT::Q::const::AID` package for different object types (e.g., auxiliary control designator type 1). While those sets share the same pattern of numeric attribute IDs, their actual values and semantics differ based on the respective object type. The AID_AUXC2 set focuses solely on the two attributes required to define a type 2 pointer and its referenced object.

## Conclusion

The AID_AUXC2 global constants provide a standardized, type-safe way to reference the attribute IDs of Auxiliary Control Designator Type 2 objects in ISOBUS applications. By encapsulating the numeric IDs as named constants, the component enhances code readability, reduces error-proneness, and ensures interoperability with the ISOBUS protocol specification.
