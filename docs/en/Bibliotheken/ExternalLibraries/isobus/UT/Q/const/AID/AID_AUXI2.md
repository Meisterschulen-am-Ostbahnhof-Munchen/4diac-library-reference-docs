# AID_AUXI2

![AID_AUXI2](./AID_AUXI2.svg)

* * * * * * * * * *
## Introduction

AID_AUXI2 is a global constant definition within the `isobus::UT::Q::const::AID` package of the 4diac-ide environment. It encapsulates a set of attribute identifiers (AIDs) specifically related to the ISO 11783 (ISOBUS) Auxiliary Input Type 2 object. These constants are used to reference individual attributes of an Auxiliary Input Type 2 object when interacting with ISOBUS virtual terminal data structures. The definition provides stable, human-readable symbolic names for the underlying attribute ID values, improving code clarity and maintainability in ISOBUS application implementations.

## Interface Structure

Since AID_AUXI2 is a global constant definition rather than a function block, adapter, or subapplication, it does not expose event/data interfaces or adapters. Instead, it declares two global constants that can be referenced by other FB types across the project.

| Constant | Type | Initial Value | Description |
|----------|------|---------------|-------------|
| `BACKGROUND_COLOUR` | `USINT` | `1` | Attribute ID 1: Background colour index of the auxiliary input type 2 object. |
| `FUNC` | `USINT` | `2` | Attribute ID 2: Bitmask encoding the auxiliary function type (bits 0–4), critical control flag (bit 5), reserved bit (bit 6, set to 0), and single-assignment flag (bit 7). |

### **Event Inputs**
- None (not applicable for a global constant definition).

### **Event Outputs**
- None (not applicable for a global constant definition).

### **Data Inputs**
- None (not applicable for a global constant definition).

### **Data Outputs**
- None (not applicable for a global constant definition).

### **Adapters**
- None (not applicable for a global constant definition).

## Functionality

The AID_AUXI2 global constants provide the numeric attribute ID values used to address attributes of an Auxiliary Input Type 2 (AUXI2) object within an ISOBUS virtual terminal. The constants enable code to refer to these attributes symbolically rather than using magic numbers:

- `BACKGROUND_COLOUR` is the attribute ID for the background colour index of the object.
- `FUNC` is the attribute ID for the function descriptor, a bitmask with the following layout:
  - Bits 0–4: Auxiliary function type (values 0–14).
  - Bit 5: Critical Control flag.
  - Bit 6: Reserved, must be set to 0.
  - Bit 7: Single-assignment flag.

These constants are intended to be used in conjunction with ISOBUS object pool manipulation APIs, where an attribute ID is required to specify which property of an object is being read or written.

## Technical Features

- **Fixed Identification**: Each constant maps directly to a standardized ISOBUS attribute ID (1 and 2 respectively).
- **Type Safety**: Constants are declared as `USINT` (unsigned short integer), matching the ISOBUS attribute ID data type.
- **Single Assignment**: Defined as `VAR_GLOBAL CONSTANT`, the values are immutable at runtime.
- **Bit-Level Semantics**: The `FUNC` constant encodes multiple flags in a single byte, requiring bitwise operations to decode or set individual bits.
- **Package Organization**: Placed in the `isobus::UT::Q::const::AID` package namespace for structured reuse across FBs.

## State Overview

A global constant definition is stateless. There is no internal state, no initialization sequence, and no runtime transitions. The values are available immediately once the containing project is loaded and remain constant throughout the execution lifetime.

## Application Scenarios

- **ISOBUS Virtual Terminal Implementation**: When constructing an Auxiliary Input Type 2 object in an object pool, developers use `AID_AUXI2.BACKGROUND_COLOUR` to set or read the background colour attribute.
- **Auxiliary Function Configuration**: The `FUNC` constant is used when configuring the functional behaviour of an auxiliary input (e.g., setting the function type, marking an input as critical, or enforcing single assignment).
- **Attribute Iteration**: The constants can be used as enumeration values when building generic attribute-handling routines for AUXI2 objects.
- **Protocol Debugging**: In diagnostic tools, the symbolic names make trace logs and breakpoints more readable than raw attribute ID numbers.

## Comparison with Similar Blocks

AID_AUXI2 is part of a family of attribute ID constant sets for different ISOBUS object types (e.g., `AID_AUXI1`, `AID_AUXI2`, `AID_*`). Compared to similar constant sets:

- It is specific to the Auxiliary Input Type 2 object, thus its attribute list covers only background colour and function descriptor.
- It does not contain attributes for labels, visibility, or other object properties that are common in other object type constant sets (e.g., Button, Output, or Input Boolean objects).
- Its `FUNC` constant provides a richer bitmask definition than simpler attribute constants, reflecting the unique capabilities of auxiliary input objects.
- Unlike FB-based implementations, it offers no processing logic, only the constant data, making it lighter and purely declarative.

## Conclusion

AID_AUXI2 is a concise and well-structured global constant definition that encapsulates the essential attribute IDs for ISOBUS Auxiliary Input Type 2 objects. By providing symbolic names for the background colour and function descriptor attributes, it enhances the readability and type safety of ISOBUS application code. Its stateless, immutable nature makes it a reliable and reusable building block within the 4diac-ide environment for agricultural machinery communication systems.