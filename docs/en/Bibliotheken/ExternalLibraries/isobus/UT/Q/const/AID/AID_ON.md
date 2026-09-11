# AID_ON

![AID_ON](./AID_ON.svg)

* * * * * * * * * *

## Introduction

The `AID_ON` set of global constants defines the attribute identifiers (IDs) used for the **Output Number Object** within the ISO 11783 (ISOBUS) User Interface (UI) standard. These constants map human‑readable names to numeric attribute codes, which are employed when reading or writing properties of an output number object on a Virtual Terminal or similar device.

This constant set is part of the `isobus::UT::Q::const::AID` package and provides a consistent way to reference object attributes without hard‑coding numeric values in application logic.

## Interface Structure

`AID_ON` is not a function block or adapter; it is a collection of constant definitions. Therefore, it does not have event or data inputs/outputs in the traditional sense. The following sections reflect the standard documentation structure for function blocks, but here they are marked as **not applicable**.

### **Event Inputs**

None – `AID_ON` defines only global constants.

### **Event Outputs**

None – `AID_ON` defines only global constants.

### **Data Inputs**

None – `AID_ON` defines only global constants.

### **Data Outputs**

None – `AID_ON` defines only global constants.

### **Adapters**

None – `AID_ON` is a constant set, not a block with adapters.

## Functionality

The `AID_ON` constants assign symbolic names to the attribute identifiers (1 to 12) used by the ISO 11783 Output Number Object. Each constant is of type `USINT` and has a fixed initial value corresponding to its attribute ID. These identifiers are used, for example, when constructing or parsing CAN messages that manipulate the properties of a number display object.

The following table summarises the constants:

| Constant Name | Value | Description |
|---------------|-------|-------------|
| `WIDTH`             | 1  | Width in pixels. |
| `HEIGHT`            | 2  | Height in pixels. |
| `BACKGROUND_COLOUR` | 3  | Background colour index. |
| `FONT_ATT`          | 4  | Object ID of a Font Attributes object. |
| `OPTIONS`           | 5  | Options bitmask. |
| `VARIABLE_REF`      | 6  | Object ID of a Number Variable object. |
| `OFFSET`            | 7  | Offset to be applied to the value for display. |
| `SCALE`             | 8  | Scale to be applied to the value for display. |
| `NUMB_DECIMALS`     | 9  | Number of decimals to display after the decimal point. |
| `FORMAT`            | 10 | Format: 0 = fixed decimal, 1 = exponential. |
| `JUSTIFICATION`     | 11 | Justification: bits 0‑1 horizontal, bits 2‑3 vertical. |
| `VALUE`             | 12 | Current value before scaling. |

## Technical Features

- **Type:** All constants are declared as `USINT` (unsigned short integer).
- **Initial Values:** Each constant is initialised with its numeric attribute ID as a `USINT#` literal.
- **Package:** The constants are defined in the package `isobus::UT::Q::const::AID`.
- **Compatibility:** Complies with the IEC 61499 standard and is intended for use in 4diac‑IDE projects interacting with ISOBUS virtual terminals.

## State Overview

`AID_ON` does not represent a state machine; it is a set of constant values. There are no states or transitions.

## Application Scenarios

- **ISOBUS Virtual Terminal Development:** The constants are used when building or interpreting commands that set or query attributes of an output number object on a Virtual Terminal.
- **HMI Design in Agricultural Machinery:** Developers can reference `AID_ON_WIDTH`, `AID_ON_HEIGHT`, etc., to configure display elements programmatically.
- **Protocol Implementation:** When constructing CAN messages according to the ISOBUS standard, these constants provide a clear and maintainable mapping.

## Comparison with Similar Blocks

`AID_ON` is a global constants collection, not a function block. Similar constant sets exist for other object types (e.g., `AID_OFF` for input number objects) and are typically named with the prefix `AID_` followed by the object type. The key difference is that `AID_ON` specifically covers attributes of the **output number** object, whereas other sets cover different object categories. Using such named constants improves code readability and reduces errors compared to using raw numerical IDs.

## Conclusion

The `AID_ON` constant set provides a clean, standardised way to refer to attribute identifiers of ISO 11783 Output Number Objects. By using these symbolic names, developers can write more maintainable and portable code for ISOBUS‑based HMI applications in the 4diac environment.
