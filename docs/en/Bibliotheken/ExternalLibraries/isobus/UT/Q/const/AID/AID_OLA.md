# AID_OLA

![AID_OLA](./AID_OLA.svg)

* * * * * * * * * *

## Introduction

`AID_OLA` is a set of global constants used in ISOBUS (ISO 11783) applications to identify attribute IDs for **Output List Objects** (OLA). It defines numeric identifiers for common attributes of a list object, such as its width, height, variable reference, and selected value. These constants are intended to be used together with the ISOBUS Virtual Terminal (VT) protocol to avoid hard-coding attribute IDs in application code, making the code more readable and maintainable.

## Interface Structure

As a **global constants** group, `AID_OLA` does not expose any event or data interface. It is not a function block, adapter, or subapplication. Instead, it provides a set of constant values that can be referenced throughout the application.

### Event Inputs
*Not applicable.*

### Event Outputs
*Not applicable.*

### Data Inputs
*Not applicable.*

### Data Outputs
*Not applicable.*

### Adapters
*Not applicable.*

## Functionality

The constants defined in `AID_OLA` correspond to the attribute identifiers used in the ISOBUS **Output List Object** (Object type 1, “Output List”). The following attributes are defined:

| Constant     | Value | Description                                                                 |
|--------------|-------|-----------------------------------------------------------------------------|
| `WIDTH`      | 1     | `AID_OLA_WIDTH` – Width of the output list in pixels.                       |
| `HEIGHT`     | 2     | `AID_OLA_HEIGHT` – Height of the output list in pixels.                     |
| `VARIABLE_REF`| 3    | `AID_OLA_VARIABLE_REF` – Object ID of a Number Variable linked to the list. |
| `VALUE`      | 4     | `AID_OLA_VALUE` – Selected list index (0 to 254), or 255 if no item is chosen. |

These constants should be used when constructing or interpreting VT command messages that manipulate an output list object.

## Technical Features

- **Data type:** All constants are of type `USINT` (unsigned short integer, 8-bit).
- **Range:** The values are fixed within the range 1–4, corresponding to the official ISOBUS attribute identifiers.
- **Package:** The constants are declared in the package `isobus::UT::Q::const::AID`.
- **Initialization:** Each constant is explicitly initialized with its numeric value using the `USINT#` prefix.
- **Compatibility:** Designed for use with 4diac IDE and ISOBUS-capable control systems.

## State Overview

Since `AID_OLA` is a constant definition, no stateful behavior exists. The values are immutable at runtime and do not change during execution.

## Application Scenarios

`AID_OLA` is typically used in applications that implement an ISOBUS Virtual Terminal client or a control function that interacts with a VT. For example:

- **Setting the size of an output list:** When creating or updating an output list object, the `WIDTH` and `HEIGHT` attributes must be transmitted with the correct IDs. Using `AID_OLA.WIDTH` and `AID_OLA.HEIGHT` avoids magic numbers.
- **Linking a variable:** The `VARIABLE_REF` constant is used when associating a number variable with the list object, enabling bidirectional data exchange.
- **Reading the user selection:** The `VALUE` constant identifies the attribute that holds the currently selected index. This allows the application to query the selected element from the VT.

## Comparison with Similar Blocks

`AID_OLA` is one of many global constant sets defined for ISOBUS object attributes. Similar groups exist for other object types, such as `AID_OUTPUT_STRING`, `AID_NUMBER_VARIABLE`, `AID_KEY`, etc. The main advantage of using such constant groups is that they provide a single source of truth for attribute IDs, reducing errors and improving code clarity. Unlike function blocks, they do not have execution logic, but they are often used together with function blocks that implement VT protocol handling.

## Conclusion

The `AID_OLA` global constants provide a clean, standardised way to reference Output List Object attribute IDs in ISOBUS applications. By using these named constants, developers can ensure correct and maintainable code when working with Virtual Terminal objects. Although it is not a functional block itself, it is an essential building block for ISOBUS communication logic.