# AID_IS

![AID_IS](./AID_IS.svg)

* * * * * * * * * *
## Introduction
The `AID_IS` global constant set defines attribute identifiers used for **Input String objects** in the context of ISOBUS (ISO 11783) Universal Terminal (UT) applications. These constants allow standardized access to the properties of an input string object, such as width, height, background colour, font attributes, and various behavioural options. They are defined as compile-time constants of type `USINT` and are intended to be referenced in function blocks or logic that interact with ISOBUS UT object pools.

## Interface Structure
This element is a global constants set and does not conform to the traditional function block interface (no events, no data I/O, no adapters). However, for documentation purposes, the constants are listed under the data section.

### **Event Inputs**
None.

### **Event Outputs**
None.

### **Data Inputs**
The following constants are provided and can be used as data sources in any function block logic:

| Name                | Type  | Initial Value | Description                                                                                                                                        |
|---------------------|-------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| `WIDTH`             | USINT | 1             | 1: AID_IS_WIDTH – Width in pixels.                                                                                                                 |
| `HEIGHT`            | USINT | 2             | 2: AID_IS_HEIGHT – Height in pixels.                                                                                                               |
| `BACKGROUND_COLOUR` | USINT | 3             | 3: AID_IS_BACKGROUND_COLOUR – Background colour index.                                                                                             |
| `FONT_ATT`          | USINT | 4             | 4: AID_IS_FONT_ATT – Object ID of a Font Attributes object.                                                                                        |
| `INP_ATT`           | USINT | 5             | 5: AID_IS_INP_ATT – Object ID of an Input Attributes / Extended Input Attributes object.                                                           |
| `OPTIONS`           | USINT | 6             | 6: AID_IS_OPTIONS – Options bitmask.                                                                                                               |
| `VARIABLE_REF`      | USINT | 7             | 7: AID_IS_VARIABLE_REF – Object ID of a String Variable object.                                                                                    |
| `JUSTIFICATION`     | USINT | 8             | 8: AID_IS_JUSTIFICATION – Justification: Bits 0-1 (Horizontal: 0=Left, 1=Middle, 2=Right), Bits 2-3 (Vertical: 0=Top, 1=Middle, 2=Bottom).        |
| `ENABLED`           | USINT | 9             | 9: AID_IS_ENABLED – 0 = Disabled, 1 = Enabled.                                                                                                     |

### **Data Outputs**
None.

### **Adapters**
None.

## Functionality
The `AID_IS` constants act as predefined attribute identifiers for an input string object within an ISOBUS UT object pool. By using these constants, application logic can unambiguously refer to specific properties (e.g., width, height, background colour, font, options, reference to a string variable, justification, and enabled state) when reading or modifying the object’s attributes via the standard ISOBUS service interfaces. The numeric values correspond to the attribute IDs defined in the ISOBUS standard for input string objects.

## Technical Features
- **Type:** All constants are of type `USINT` (unsigned 8-bit integer).
- **Values:** The identifiers are sequential from 1 to 9, matching the official ISOBUS specification for input string object attributes.
- **Compile-time constants:** Declared as `VAR_GLOBAL CONSTANT`, they are resolved during compilation, offering performance benefits and guaranteeing immutability.
- **Namespace:** The constants belong to the package `isobus::UT::Q::const::AID`, ensuring a clear hierarchical organisation.

## State Overview
This global constant set does not maintain any state. It provides static values that are available globally throughout the application. There are no runtime modifications, and the constants remain fixed for the entire execution.

## Application Scenarios
The `AID_IS` constants are used in IEC 61499 applications that interact with ISOBUS Universal Terminal objects, particularly when a task requires:
- Configuring an input string object’s dimensions or appearance.
- Changing the font or background colour.
- Setting or retrieving the associated string variable.
- Adjusting text justification or enabling/disabling the input field.
- Reading or writing options bitmask values.

For example, a function block controlling a text entry field might use `AID_IS.WIDTH` and `AID_IS.HEIGHT` to specify the size in its object pool creation request.

## Comparison with Similar Blocks
While this element is not a function block, it is comparable to other **attribute ID constant sets** defined in the same package (e.g., `AID_IS` for input strings, or analogous sets for Output String, Number, etc.). The key difference is that each set defines attribute IDs for a specific object type, allowing developers to use mnemonic names instead of raw numeric values, reducing errors and improving code readability. Unlike function blocks, it provides no algorithmic functionality; it purely supplies constant data.

## Conclusion
The `AID_IS` global constant set is a vital building block for ISOBUS UT applications, offering a clear and standardized way to reference attributes of input string objects. Its use enhances maintainability and compliance with the ISOBUS standard, making it an essential component for developers working with the 4diac-ide environment in agricultural or mobile machinery contexts.