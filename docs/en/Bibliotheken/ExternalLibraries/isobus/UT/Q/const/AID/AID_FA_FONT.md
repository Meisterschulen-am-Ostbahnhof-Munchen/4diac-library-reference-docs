# AID_FA_FONT

![AID_FA_FONT](./AID_FA_FONT.svg)

* * * * * * * * * *

## Introduction

This global constant type defines the attribute identifiers (AID) used for the **Font Attributes Object** within the ISOBUS Universal Terminal (UT) protocol. The constants map to specific attribute indices that are used to access and modify font properties on a virtual terminal. These IDs are essential for any IEC 61499 application that communicates with ISOBUS compliant displays.

## Interface Structure

As a global constant type, `AID_FA_FONT` does not provide event or data inputs/outputs like function blocks. Instead, it exposes a set of named constants that can be referenced throughout the project. The constants are defined as unsigned short integers (USINT) and are listed below.

### **Event Inputs**

Not applicable – this type does not process events.

### **Event Outputs**

Not applicable – this type does not generate events.

### **Data Inputs**

The type defines the following constants, which can be used directly in IEC 61499 logic:

| Constant Name | Type | Value | Description |
|---------------|------|-------|-------------|
| `COLOUR`      | USINT | 1     | Attribute ID for the font colour index. |
| `SIZE`        | USINT | 2     | Attribute ID for the font size. |
| `FONT_TYPE`   | USINT | 3     | Attribute ID for the font type (e.g., sans‑serif, serif). |
| `STYLE`       | USINT | 4     | Attribute ID for the font style – a bitmask (see details below). |

### **Data Outputs**

Not applicable – constants are not outputs.

### **Adapters**

Not applicable – this type does not use adapters.

## Functionality

The `AID_FA_FONT` constants are used to address specific attributes of the `Font Attributes Object` in the ISOBUS object dictionary. When an application needs to read or write a font property, it passes the corresponding constant as the attribute identifier in the service request. This ensures correct and consistent access across different implementations.

The **STYLE** attribute is a bitmask with the following meanings per bit (bit 0 is least significant):

| Bit | Meaning                     |
|-----|-----------------------------|
| 0   | Bold                        |
| 1   | Crossed Out                 |
| 2   | Underlined                  |
| 3   | Italic                      |
| 4   | Inverted                    |
| 5   | Flashing inverted           |
| 6   | Flashing hidden             |
| 7   | Proportional rendering      |

## Technical Features

- All constants are of type `USINT` (unsigned 8‑bit integer).
- Values are predefined and must not be changed at runtime.
- The constants are grouped under the package `isobus::UT::Q::const::AID`.
- They follow the ISOBUS 61499‑1 standard for object attributes.
- The type is intended to be used with the ISOBUS UT communication stack.

## State Overview

Not applicable – the type does not contain state machines or dynamic behavior.

## Application Scenarios

- **Setting font properties on a Virtual Terminal** – When a client application wants to change the font of a label or text object, it uses the appropriate constant (e.g., `AID_FA_FONT::COLOUR`) to specify the attribute to modify.
- **Reading font attributes** – To retrieve the current font settings, the same attribute IDs are used in read requests.
- **Building ISOBUS diagnostic tools** – Developers can use the well‑defined constants to implement testing or monitoring applications for UT communication.

## Comparison with Similar Blocks

Unlike function blocks that process data or events, `AID_FA_FONT` is a read‑only constant type. Similar global constant types exist for other object attribute IDs (e.g., `AID_FA_*` for different object types) and are part of the same `isobus::UT::Q::const` package. The distinguishing feature of this type is its specific set of attribute identifiers for font objects, ensuring standardised naming and values.

## Conclusion

`AID_FA_FONT` provides a standardised and maintainable way to reference font attribute IDs in ISOBUS UT applications. By using these constants, developers avoid hard‑coded magic numbers, reduce errors, and improve code clarity. The type is an essential building block for any IEC 61499 application that interacts with ISOBUS virtual terminals.
