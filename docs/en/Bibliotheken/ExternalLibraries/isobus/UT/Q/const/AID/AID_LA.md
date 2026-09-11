# AID_LA

![AID_LA](./AID_LA.svg)

* * * * * * * * * *
## Introduction

AID_LA is a set of global constants that define the attribute identifiers for the ISO 11783 (ISOBUS) **Line Attributes Object**. These constants are used to address specific properties of a line object within a Virtual Terminal (VT) – primarily the line colour, line width, and line art. The constants are defined as `USINT` (Unsigned Short Integer) values and are intended to be used as object attribute IDs when reading or writing line properties via the ISOBUS protocol.

The values correspond to the standard attribute IDs as specified in the ISOBUS VT standard:
- `1` – Line Colour
- `2` – Line Width
- `3` – Line Art (bit pattern for line style)

These constants simplify the development of VT applications by providing a human‑readable symbolic name for each attribute ID, avoiding magic numbers in the application logic.

## Interface Structure

Since AID_LA is a global constants container (not a function block or adapter), it does not have event inputs, event outputs, data inputs, data outputs, or adapters. Instead, it provides three global constants that can be referenced from any part of an IEC 61499 application.

### **Event Inputs**

None – AID_LA defines only constants, no event inputs.

### **Event Outputs**

None – AID_LA defines only constants, no event outputs.

### **Data Inputs**

None – AID_LA defines only constants, no data inputs.

### **Data Outputs**

None – AID_LA defines only constants, no data outputs.

### **Adapters**

None – AID_LA is not an adapter type.

## Functionality

The constant container `AID_LA` provides symbolic names for the three line attribute identifiers. The constants are defined as:

| Constant Name  | Type   | Value  | Description                                                                |
|----------------|--------|--------|----------------------------------------------------------------------------|
| `LINE_COLOUR`  | `USINT`| `1`    | Attribute ID for line colour (AID_LA_LINE_COLOUR)                          |
| `LINE_WIDTH`   | `USINT`| `2`    | Attribute ID for line width (AID_LA_LINE_WIDTH)                            |
| `LINE_ART`     | `USINT`| `3`    | Attribute ID for line art – a bit pattern where each bit represents a paintbrush spot (AID_LA_LINE_ART) |

These constants are intended to be used in conjunction with VT line objects. For example, when setting the colour of a line, an application would use `AID_LA.LINE_COLOUR` as the attribute ID in the appropriate ISO 11783 command.

## Technical Features

- **Data Type**: All constants are of type `USINT` (unsigned 8‑bit integer), sufficient for the attribute IDs.
- **Namespace**: The constants are declared inside a package `isobus::UT::Q::const::AID`, ensuring a clear naming structure for reuse in larger projects.
- **Initialisation**: Each constant is explicitly initialised with a literal value (`USINT#1`, `USINT#2`, `USINT#3`), making the assignment unambiguous.
- **IEC 61499 Compatibility**: The constants are defined as `VAR_GLOBAL CONSTANT`, which is the IEC 61499 mechanism for global constants, accessible from any FB network.

## State Overview

AID_LA is a static, immutable container. It has no internal states, no execution logic, and no lifecycle. The constants are available at compile time and remain unchanged during runtime.

## Application Scenarios

Typical usage scenarios include:

- **VT Screen Design**: When building a VT page that contains a line object, the developer needs to specify the line colour, width, or style. Instead of using numeric codes directly, the developer can reference these constants for better readability and maintainability.
- **Runtime Attribute Configuration**: In a control application that dynamically changes a line’s appearance (e.g., highlighting a boundary), the constant is used as the attribute ID in the `SetLineAttributes` or similar. This ensures the correct attribute is addressed.
- **Diagnostics and Logging**: When logging VT object properties, the symbolic name can be used for clarity.

## Comparison with Similar Blocks

In the ISOBUS context, there are other attribute ID constant sets, for example:
- **AID_OO** – Object identifiers for object attributes.
- **AID_PO** – Point attributes.
- **AID_AO** – Auxiliary Object attributes.

AID_LA is specifically focused on line attributes. Compared to equivalent numeric literals, the constant set offers the advantage of:
- **Readability**: `AID_LA.LINE_COLOUR` is self‑documenting.
- **Maintainability**: If the underlying ID ever changes (unlikely), the definition can be updated in a single place.
- **Type Safety**: Using `USINT` ensures the value fits the expected range for attribute IDs.

## Conclusion

The `AID_LA` global constants provide a clean and standardised way to referencing line attribute IDs in ISOBUS VT applications. They are simple, well‑defined, and align with the ISO 11783 standard. By using these constants, developers reduce the risk of errors and improve the clarity of their code, making the implementation of VT line objects more straightforward and robust.