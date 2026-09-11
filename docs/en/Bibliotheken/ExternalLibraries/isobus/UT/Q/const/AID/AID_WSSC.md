# AID_WSSC

![AID_WSSC](./AID_WSSC.svg)

* * * * * * * * * *

## Introduction

The `AID_WSSC` global constants definition provides a set of predefined attribute identifiers for **Working Set Special Controls** objects in the ISO 11783 (ISOBUS) environment. These constants are used to reference specific attributes when interacting with such objects in a tractor-implement control system. The definition is part of the `isobus::UT::Q::const::AID` package, enabling consistent and readable addressing of these attributes across different parts of an industrial automation application.

## Interface Structure

This element is a **global constants** block, not a conventional function block. As such, it does not have any event inputs, event outputs, or adapters. Instead, it exposes a set of constant values that can be directly referenced in the application logic.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

| Constant Name   | Data Type | Initial Value | Comment                                                                 |
|-----------------|-----------|---------------|-------------------------------------------------------------------------|
| `NUMOFBYTES`    | `USINT`   | `USINT#1`     | Number of bytes to follow in this object (attribute ID 1).              |
| `COLOUR_MAP`    | `USINT`   | `USINT#2`     | Object ID of a Colour Map object, or NULL (attribute ID 2).             |
| `COLOUR_PALETTE`| `USINT`   | `USINT#3`     | Object ID of a Colour Palette object, or NULL (attribute ID 3).         |

These constants are read-only and can be used anywhere in the program to refer to the corresponding attribute IDs.

### **Adapters**

None.

## Functionality

The `AID_WSSC` constants define the attribute identifiers used for the **Working Set Special Controls** object as specified by the ISO 11783 standard. In practice, these constants are used when constructing or interpreting ISOBUS messages that deal with working set special controls, such as setting or retrieving colour map or palette information. By using these named constants, the code becomes self-documenting and avoids magic numbers.

The values are fixed at:

- `NUMOFBYTES` = 1 (the attribute that indicates the number of bytes following in the object)
- `COLOUR_MAP` = 2 (the attribute that stores the object identifier of a colour map, or NULL)
- `COLOUR_PALETTE` = 3 (the attribute that stores the object identifier of a colour palette, or NULL)

## Technical Features

- **Data Type:** All constants are of type `USINT` (unsigned short integer, 8-bit).
- **Initial Values:** Explicitly set to `USINT#1`, `USINT#2`, and `USINT#3` respectively, ensuring deterministic behaviour.
- **Package:** Declared in `isobus::UT::Q::const::AID`, which organizes the constant group under a dedicated namespace.
- **Compatibility:** Conforms to the IEC 61499 standard for global constants, making it usable in 4diac-IDE and other compliant environments.

## State Overview

This block contains no state variables and does not maintain any runtime state. The constants are always available and immutable during the execution of the application. Therefore, no state diagram is applicable, and the behaviour is consistent throughout the entire runtime.

## Application Scenarios

- **ISOBUS Object Pool Management:** When a working set special controls object is created or modified, the corresponding attribute IDs can be set using these constants, e.g., `AID_WSSC.NUMOFBYTES` to specify the length of the object.
- **Colour Configuration:** The `COLOUR_MAP` and `COLOUR_PALETTE` constants are used to assign colour resources to working set special controls, enabling a consistent visual representation on the virtual terminal.
- **Protocol Message Construction:** In communication tasks, these constants help to construct or parse messages that carry attribute information, improving code readability and reducing the risk of logical errors.

## Comparison with Similar Blocks

Similar global constant definitions exist for other ISOBUS object types, such as:

- `AID_WSC` (Working Set Controls)
- `AID_VC` (Virtual Terminal Controls)
- `AID_IS` (Input/Output Objects)

While those also provide numeric attribute IDs as constants, `AID_WSSC` specifically targets the *Working Set Special Controls* object. Its constants are tailored to the attributes defined for that object type, making it distinct in purpose and usage. In contrast to ordinary function blocks, this global constants block does not execute any logic and does not participate in data or event flow; it simply offers named integer values.

## Conclusion

The `AID_WSSC` global constants block is a simple but essential component for IEC 61499 and ISOBUS applications. By encapsulating attribute identifiers in named constants, it enhances code maintainability and reduces the chance of errors when dealing with working set special controls. Its integration into the 4diac-IDE environment follows standard practice and allows for direct reuse across projects. As a static definition, it serves as a reliable reference for developers implementing ISOBUS-compliant functionality.
