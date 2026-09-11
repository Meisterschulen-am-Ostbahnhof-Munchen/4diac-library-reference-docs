# AID_AUXF2

![AID_AUXF2](./AID_AUXF2.svg)

* * * * * * * * * *

## Introduction

AID_AUXF2 is a `GLOBALCONSTANTS` type in 4diac-ide that defines the object attribute IDs of an ISO 11783 ISOBUS Auxiliary Function Type 2 (AUX-F2) object. It is not an executable function block, but a compile-time constant container that makes object attribute IDs available as named symbols. The constants are part of the namespace `isobus::UT::Q::const::AID`.

## Interface Structure

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

AID_AUXF2 does not have a dynamic IEC 61499 interface with event or data ports. Instead, it provides the following global constants:

| Constant | Type | Value | Description |
| --- | --- | --- | --- |
| `BACKGROUND_COLOUR` | `USINT` | `USINT#1` | Background colour index attribute ID. |
| `FUNC` | `USINT` | `USINT#2` | Function attribute ID. Value is a bitmask: bits 0-4 = auxiliary function type (0-14), bit 5 = critical control, bit 6 = assignment restriction, bit 7 = single-assignment. |

## Functionality

The purpose of AID_AUXF2 is to provide named constants for the attribute IDs used by an Auxiliary Function Type 2 object. The two constants allow application code to refer to AUX-F2 attributes without using hard-coded integer values.

- `BACKGROUND_COLOUR` identifies the background colour index attribute.
- `FUNC` identifies the auxiliary function attribute and describes the function as a bitmask.

Because these values are global constants, they can be used in expressions, comparisons, or case selections without requiring runtime initialization or additional storage.

## Technical Features

- Defined as `GLOBALCONSTANTS`, not as an executable function block.
- Both constants are of type `USINT`.
- `BACKGROUND_COLOUR` has the fixed value `1`.
- `FUNC` has the fixed value `2`.
- The constants are available in the namespace `isobus::UT::Q::const::AID`.
- The container has no events, data inputs, data outputs, adapters, or algorithms.
- The `FUNC` attribute uses bits 0-4 for the auxiliary function type, bit 5 for critical control, bit 6 for assignment restriction, and bit 7 for single-assignment.

## State Overview

Not applicable. A `GLOBALCONSTANTS` type has no ECC state machine, no execution states, and no runtime behavior. The constants are static and are available to all IEC 61499 resources that reference this definition.

## Application Scenarios

- **ISOBUS Virtual Terminal applications**: Use `AID_AUXF2.BACKGROUND_COLOUR` and `AID_AUXF2.FUNC` as attribute IDs when working with Auxiliary Function Type 2 objects.
- **Object pool generation**: Reference the constants when creating or modifying AUX-F2 object attributes in an ISOBUS object pool.
- **Readability improvement**: Replace numeric attribute IDs with symbolic names to reduce errors and improve code maintainability.

Example usage:

```iec61499
IF attrID = AID_AUXF2.FUNC THEN
    (* Process auxiliary function bitmask *)
END_IF
```

## Comparison with Similar Blocks

AID_AUXF2 is not a function block and therefore cannot be compared with function blocks in terms of events, states, or data flow. It is best compared with other global constant containers that define ISOBUS object attribute IDs. Similar containers exist for other auxiliary function object types, such as Auxiliary Function Type 1 or Type 3. The main difference is the object type being addressed and the set of attribute IDs defined by each container. AID_AUXF2 is specifically dedicated to the Auxiliary Function Type 2 object and defines the two attribute IDs relevant for this object.

## Conclusion

AID_AUXF2 is a compact global constant definition that provides named attribute IDs for ISOBUS Auxiliary Function Type 2 objects. By defining `BACKGROUND_COLOUR` and `FUNC` as named constants, it improves code clarity, reduces the use of magic numbers, and supports maintainable VT application development in 4diac-ide.