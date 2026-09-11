# AID_OS

![AID_OS](./AID_OS.svg)

* * * * * * * * * *

## Introduction

The **AID_OS** global constant set defines the attribute identifiers (IDs) for the **Output String (OS) object** within the ISO 11783 (ISOBUS) protocol, specifically for the Working Set (WS) and Virtual Terminal (VT) layers. These IDs are used to reference specific attributes of an output string object when interacting with the object pool via the ISOBUS service primitives. The set is part of a larger collection of constant definitions that simplify and standardize the handling of object attributes in 4diac-based ISOBUS applications.

The constants are of type `USINT` (unsigned short integer) and range from 1 to 7, each corresponding to a unique attribute of the output string object, such as width, height, background color, font attributes, options, variable reference, and justification.

## Interface Structure

This global constant set does not represent a function block, adapter, or subapplication. Instead, it provides a set of named constants that can be used directly in IEC 61499 applications to access the respective attribute IDs. The “interface” of the set is therefore the list of constants it exposes.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None directly. However, the constants serve as data values that can be passed to function blocks or functions that require an attribute ID.

### **Data Outputs**

The following global constants are defined and can be referenced anywhere in the application:

| Constant Name        | Value | Description |
|----------------------|-------|-------------|
| `WIDTH`             | `USINT#1` | 1: `AID_OS_WIDTH` – Width in pixels. |
| `HEIGHT`            | `USINT#2` | 2: `AID_OS_HEIGHT` – Height in pixels. |
| `BACKGROUND_COLOUR` | `USINT#3` | 3: `AID_OS_BACKGROUND_COLOUR` – Background colour index. |
| `FONT_ATT`          | `USINT#4` | 4: `AID_OS_FONT_ATT` – Object ID of a Font Attributes object. |
| `OPTIONS`           | `USINT#5` | 5: `AID_OS_OPTIONS` – Bitmask: Bit 0 = Transparent, Bit 1 = Auto-Wrap, Bit 2 = Wrap on Hyphen. |
| `VARIABLE_REF`      | `USINT#6` | 6: `AID_OS_VARIABLE_REF` – Object ID of a Variable object. |
| `JUSTIFICATION`     | `USINT#7` | 7: `AID_OS_JUSTIFICATION` – Justification: Bits 0-1 (Horizontal: 0=Left, 1=Middle, 2=Right), Bits 2-3 (Vertical: 0=Top, 1=Middle, 2=Bottom). |

### **Adapters**

None.

## Functionality

The `AID_OS` constant set is used to map symbolic names to numeric attribute IDs for an ISOBUS Output String object. When an application needs to read or write an attribute of such an object (e.g., via the `Set_Attribute` or `Get_Attribute` services), it can use these constants instead of hard-coded numeric values. This improves code readability, maintainability, and reduces the risk of errors.

The constants are declared as global constants within the namespace `isobus::UT::Q::const::AID`, ensuring they are globally accessible and immutable. They are compiled directly into the application and occupy no runtime memory beyond the constant literals.

## Technical Features

- **Type**: `USINT` (unsigned 8-bit integer)
- **Range**: 1 to 7 (inclusive)
- **Namespace**: `isobus::UT::Q::const::AID`
- **Immutability**: The constants are declared as `CONSTANT` and cannot be modified at runtime.
- **Direct Usage**: May be used in any IEC 61499 FB or ST code without further instantiation.
- **Documentation**: Each constant is accompanied by a comment describing its corresponding ISOBUS `AID_OS_*` definition.

## State Overview

Not applicable. This is a constant set without internal states or dynamic behavior.

## Application Scenarios

Typical use cases for `AID_OS` include:

- **ISO 11783 Virtual Terminal Implementations**: When developing a VT client or server that manages output string objects, these constants are used to construct attribute access commands.
- **Object Pool Manipulation**: When reading or modifying object attributes in an ISOBUS object pool, the constants provide a clear mapping.
- **Diagnostic and Monitoring Tools**: Applications that inspect or alter output string properties (e.g., text alignment, transparency, size) can use these IDs to identify the target attribute.
- **Integration with 4diac Function Blocks**: In 4diac, function blocks that handle ISOBUS communication can accept these constants as inputs to specify which attribute to target.

## Comparison with Similar Blocks

In the same ISOBUS context, other constant sets exist for different object types, such as:

- `AID_OBJ` – general object attributes
- `AID_AB` – for Alphanumeric Input objects
- `AID_WS` – for Working Set objects
- `AID_OS` is specifically tailored to the Output String object.

Unlike a function block, `AID_OS` does not execute logic or contain state; it is purely a data definition. It serves as a companion to function blocks that implement the ISOBUS communication services, providing a consistent and readable way to reference attribute IDs.

## Conclusion

`AID_OS` is a compact and essential set of global constants that encapsulate the attribute identifiers of the ISOBUS Output String object. By using these symbolic constants, developers can write clearer, more maintainable 4diac applications that interact with ISOBUS VTs. The set is well-documented, follows the standard naming convention, and aligns with the Eclipse 4diac ecosystem's philosophy of modular and reusable components.
