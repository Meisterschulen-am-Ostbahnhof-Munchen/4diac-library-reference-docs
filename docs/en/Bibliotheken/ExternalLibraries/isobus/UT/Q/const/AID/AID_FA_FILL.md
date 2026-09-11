# AID_FA_FILL

![AID_FA_FILL](./AID_FA_FILL.svg)

* * * * * * * * * *
## Introduction

The `AID_FA_FILL` global constant set defines attribute identifiers for the **Fill Attributes Object** within the ISO 11783 (ISOBUS) protocol. These constants are used to reference specific fill properties inside a fill attributes object, enabling consistent and readable access to fill type, colour, and pattern settings across ISOBUS applications. The constants are defined as `USINT` (unsigned short integer) values and are intended to be used with the ISOBUS virtual terminal implementation.

## Interface Structure

Since `AID_FA_FILL` is a global constants block, it does not contain event or data inputs/outputs in the conventional function block sense. Instead, it exposes three constant values that can be referenced globally within your IEC 61499 application.

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

The following table summarises the constants provided by this global constants block:

| Constant Name | Type | Value | Description |
|---------------|------|-------|-------------|
| `FILL_TYPE`   | `USINT` | `1` | Attribute ID 1: Fill type. Values: 0 = no fill, 1 = fill with line colour, 2 = fill with specified colour, 3 = fill with pattern. |
| `COLOUR`      | `USINT` | `2` | Attribute ID 2: Colour index used for filling. |
| `PATTERN`     | `USINT` | `3` | Attribute ID 3: Pattern index used for filling. |

These constants are defined as `CONSTANT` in the `VAR_GLOBAL_CONSTANT` section, meaning they are immutable and globally accessible.

## Functionality

The constants in `AID_FA_FILL` are used to address fields within an ISOBUS Fill Attributes object. In ISOBUS, objects like fill attributes are stored as records with indexed elements. By using these predefined attribute IDs, application logic can reliably refer to the correct data element without hard-coding numeric values, improving readability and maintainability.

For example, to set the fill type of a fill attributes object, an application would use `AID_FA_FILL.FILL_TYPE` as the attribute identifier, and the corresponding value would be one of the allowed fill type codes.

## Technical Features

- **Global Constants**: The definitions are located in a global constants block, making them available to all function blocks and resources in the 4diac IDE project.
- **Data Type**: All constants are of type `USINT` (unsigned 8-bit integer), which is sufficient for the small enumeration values and colour indices used.
- **Constant Declaration**: The `CONSTANT` keyword ensures that these values cannot be modified at runtime, providing safety and preventing accidental changes.
- **Standard Compliance**: The constants follow the ISOBUS standard for attribute IDs, specifically for the Fill Attributes Object.

## State Overview

This global constants block has no internal states or state machines; it is purely a declarative element.

## Application Scenarios

The `AID_FA_FILL` constants are typically used in applications that interact with the ISOBUS Virtual Terminal (VT) or implement ISOBUS-compliant functions. Example use cases include:

- Setting up a fill attributes object in a VT display.
- Modifying the fill type of a graphic element (e.g., a bar graph or gauge) based on user input or process conditions.
- Writing colour or pattern selections to a VT object.

## Comparison with Similar Blocks

There are other global constant blocks in the ISOBUS library, such as `AID_FA_LINE` or `AID_FA_TEXT`, which define attribute IDs for line and text attributes respectively. These follow the same pattern: a set of `USINT` constants representing attribute indices. Compared to those, `AID_FA_FILL` focuses exclusively on fill-related attributes, providing a clear separation of concerns.

## Conclusion

The `AID_FA_FILL` global constant block provides a clean, standards-based way to reference fill attribute IDs in ISOBUS applications. By using these constants, developers can avoid magic numbers, improve code clarity, and ensure alignment with the ISOBUS specification. It is a fundamental building block for any function block that needs to manage fill attributes on a virtual terminal.