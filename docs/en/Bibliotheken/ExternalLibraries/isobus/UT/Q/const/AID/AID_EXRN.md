# AID_EXRN

![AID_EXRN](./AID_EXRN.svg)

* * * * * * * * * *

## Introduction

AID_EXRN is an IEC 61499 global constants container defined for the 4diac-IDE. It provides symbolic constant values for the object attribute identifiers used by the external reference name object. This XML type is not a regular executable function block; it is a global constants class intended to be shared across an application or project. It is part of the package `isobus::UT::Q::const::AID`.

## Interface Structure

### **Event Inputs**

None. AID_EXRN does not define any event inputs.

### **Event Outputs**

None. AID_EXRN does not define any event outputs.

### **Data Inputs**

None. AID_EXRN does not define any data inputs.

### **Data Outputs**

None. AID_EXRN does not define any data outputs.

### **Adapters**

None. AID_EXRN does not define any adapters.

## Functionality

AID_EXRN defines a set of constant attribute identifiers for the external reference name object. These constants allow algorithms to use meaningful symbolic names instead of raw numeric values.

The following constants are declared:

| Constant | Value | Meaning |
|---|---|---|
| `OPTIONS` | `USINT#1` | Attribute ID `1`: `AID_EXRN_OPTIONS`; bitmask with bit 0 = Enabled, allowing external reference by NAME WS. |
| `NAME_0` | `USINT#2` | Attribute ID `2`: `AID_EXRN_NAME_0`. |
| `NAME_1` | `USINT#3` | Attribute ID `3`: `AID_EXRN_NAME_1`. |

These values are fixed at global constant declaration time and can be referenced from other IEC 61499 elements such as algorithms and expressions.

## Technical Features

- The XML root element is `GlobalConstants`, not a regular function block type.
- All declared constants use the `USINT` data type.
- The constants are declared with initial values and are globally available.
- The package name is `isobus::UT::Q::const::AID`.
- The compiler package is also `isobus::UT::Q::const::AID`.
- The constants are intended for ISOBUS-related use cases, specifically for external reference name attribute handling.

## State Overview

AID_EXRN has no execution state machine. Because it is a global constants class, it does not contain `START`, `STOP`, or any internal algorithmic states. It is passive and simply provides constant data to the surrounding application.

## Application Scenarios

AID_EXRN is useful in applications that need to interact with ISOBUS external reference name objects. Typical use cases include:

- Setting or reading attributes of an external reference name object.
- Checking or configuring the `OPTIONS` bitmask to enable external references by NAME WS.
- Writing maintainable algorithms that use symbolic constants instead of hard-coded attribute IDs.

## Comparison with Similar Blocks

A regular function block typically has event inputs, event outputs, data inputs, data outputs, and internal state. AID_EXRN has none of these. It is comparable to other IEC 61499 global constants groups, but it is specialized for external reference name attribute IDs. This makes it similar to a shared enumeration or constant header in conventional programming languages.

## Conclusion

AID_EXRN is a small, focused global constants definition that centralizes the attribute IDs used by the external reference name object. It improves readability, maintainability, and consistency in ISOBUS-related 4diac-IDE applications.
