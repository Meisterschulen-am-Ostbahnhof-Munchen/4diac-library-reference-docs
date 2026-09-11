# AID_EXOP

![AID_EXOP](./AID_EXOP.svg)

* * * * * * * * * *

## Introduction

`AID_EXOP` is a global constant definition used in the IEC 61499 (4diac-ide) environment. It defines attribute identifiers for the **External Object Pointer** object type within the ISOBUS (ISO 11783) protocol stack. These constants are used to reference specific attributes of external object pointers in a standardized, human-readable way, replacing raw numeric IDs in application code.

This definition is part of a package named `isobus::UT::Q::const::AID` and is intended to be used as a shared set of constants across multiple function blocks or applications. The constant values are defined as `USINT` (unsigned short integer) literals.

## Interface Structure

Since `AID_EXOP` is a global constant definition, it does **not** expose any event, data, or adapter interfaces. It is not a function block or subapplication instance; it merely provides constant values that can be referenced by other FB types.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None. The variables defined here are compile-time constants, not runtime data inputs.

### **Data Outputs**

None.

### **Adapters**

None.

## Global Constants

The following table lists all constants defined by `AID_EXOP`:

| Constant Name     | Type    | Value   | Description                                                                 |
|-------------------|---------|---------|-----------------------------------------------------------------------------|
| `DEF_OBJ_ID`      | `USINT` | `USINT#1` | Default Object ID to display if an external ID is not valid or is NULL. This corresponds to the attribute `AID_EXOP_DEF_OBJ_ID`. |
| `EXT_REF_NAME_ID` | `USINT` | `USINT#2` | Attribute ID for the external reference name. Corresponds to `AID_EXOP_EXT_REF_NAME_ID`. |
| `EXT_OBJ`         | `USINT` | `USINT#3` | Attribute ID for the external object itself. Corresponds to `AID_EXOP_EXT_OBJ`. |

These constants are declared as `VAR_GLOBAL CONSTANT` and are therefore immutable and accessible from any FB or function within the same project that includes this definition.

## Functionality

The primary purpose of `AID_EXOP` is to provide a consistent set of attribute ID constants for the ISOBUS **External Object Pointer** object. In ISOBUS implementations, object pools and virtual terminals exchange object attribute data using numeric identifiers. Using named constants improves code readability and maintainability, and reduces the risk of typographical errors.

When used in conjunction with other ISOBUS-related function blocks (e.g., those handling object pool management or VT communication), these constants are referenced to read or modify external object pointer attributes, such as the default object ID, reference name, or the external object handle.

## Technical Features

- **Type:** `USINT` (unsigned 8-bit integer), suitable for attribute IDs in the ISOBUS standard (range 0–255).
- **Initialization:** Each constant has an explicit initializer using the `USINT#` literal syntax.
- **Persistence:** Declared as `VAR_GLOBAL CONSTANT`, meaning the values are fixed at compile time and cannot be changed at runtime.
- **Package:** Located in `isobus::UT::Q::const::AID`, indicating its association with ISOBUS utility and constant definitions.
- **Compatibility:** The constants follow the naming and numbering conventions of the ISOBUS AID (Attribute ID) specification, allowing direct mapping to protocol-level identifiers.

## State Overview

This definition has no runtime state. It is a purely static set of constants. There are no state transitions, initialization procedures, or execution steps associated with it. The values are available as soon as the containing application or library is loaded.

## Application Scenarios

`AID_EXOP` is typically used in ISOBUS applications where external object pointers are managed:

- **Virtual Terminal (VT) implementations:** Referencing the default object ID or external object attributes when building or updating object pools.
- **Task controllers:** Handling external objects like implements or sensors, where pointer attributes need to be queried or set.
- **Diagnostic and status display:** Determining which object to display when an external ID is invalid or NULL.
- **Protocol adaptation:** Mapping internal data structures to ISOBUS attribute IDs for communication with other ISOBUS nodes.

Example usage in ST (Structured Text):

```pascal
// Use the default object ID constant
myObject := DEF_OBJ_ID;
```

## Comparison with Similar Blocks

Similar global constant definitions in ISOBUS contexts are typically created for other object types, such as `AID_OBJ`, `AID_OP`, or `AID_DP`. These follow the same pattern:

- `AID_EXOP` – External Object Pointer attribute IDs
- `AID_OP` – Object Pointer attribute IDs
- `AID_DP` – Device Process Data attribute IDs

The main difference between these definitions is the set of attributes they contain and their numeric values. `AID_EXOP` specifically addresses the three attributes listed above, whereas other definitions may have more entries. All such constant sets serve the same purpose: providing named references to numeric protocol identifiers, improving code clarity and reducing maintenance effort.

## Conclusion

`AID_EXOP` is a compact but essential set of global constants for ISOBUS external object pointer attribute IDs. While it has no executable interface, it plays a key role in making PLC and industrial automation code more readable and robust. By using these named constants instead of raw numbers, developers can avoid errors and ensure compliance with the ISOBUS protocol specification. Its integration into 4diac-based projects is straightforward, and it forms part of a broader library of protocol-related constants.
