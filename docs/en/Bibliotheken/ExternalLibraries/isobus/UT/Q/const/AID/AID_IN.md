# AID_IN

![AID_IN](./AID_IN.svg)

* * * * * * * * * *
## Introduction

AID_IN is a global constants container used in 4diac-ide for ISO 11783 / ISOBUS applications. It defines the numeric attribute IDs for an Input Number object on a Virtual Terminal. The provided XML declares named constants of type `USINT` with values from `1` to `15`, allowing the attribute IDs to be referenced by meaningful names instead of magic numbers.

Although the XML structure resembles an IEC 61499 type definition, AID_IN is not a function block, adapter, or subapplication. It is a compile-time constants definition intended for use in ISOBUS-related logic.

## Interface Structure

The AID_IN element does not define an IEC 61499 function block interface. It contains no event inputs, event outputs, data inputs, data outputs, or adapters. Its complete interface is represented by the global constants described in the Functionality section.

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

## Functionality

AID_IN provides the attribute identifiers for an ISOBUS Input Number object. Each constant represents a specific attribute ID that can be used when reading from or writing to an ISOBUS Virtual Terminal.

The following table lists all defined constants:

| Constant | Value | Description |
|---|---|---|
| `WIDTH` | `USINT#1` | Width in pixels. |
| `HEIGHT` | `USINT#2` | Height in pixels. |
| `BACKGROUND_COLOUR` | `USINT#3` | Background colour index. |
| `FONT_ATT` | `USINT#4` | Object ID of a Font Attributes object. |
| `OPTIONS` | `USINT#5` | Bitmask: Bit 0 = Transparent, Bit 1 = Display leading zeros, Bit 2 = Display zero as blank, Bit 3 = Truncate decimals. |
| `VARIABLE_REF` | `USINT#6` | Object ID of a Variable object. |
| `MIN_VALUE` | `USINT#7` | Minimum value. |
| `MAX_VALUE` | `USINT#8` | Maximum value. |
| `OFFSET` | `USINT#9` | Offset. |
| `SCALE` | `USINT#10` | Scale. |
| `NUMB_DECIMALS` | `USINT#11` | Number of decimals. |
| `FORMAT` | `USINT#12` | Decimal display format. `0` means fixed format decimal, `1` means exponential format. |
| `JUSTIFICATION` | `USINT#13` | Justification. Bits 0-1 define horizontal alignment, bits 2-3 define vertical alignment. |
| `VALUE` | `USINT#14` | Current value. |
| `OPTIONS_2` | `USINT#15` | Bitmask: Bit 0 = Enabled, Bit 1 = Real-time editing. |

## Technical Features

- Declared as an XML `<GlobalConstants>` element.
- Defined in the package namespace `isobus::UT::Q::const::AID`.
- All constants use the `USINT` data type.
- Each constant has an initial value equal to its corresponding ISOBUS attribute ID.
- The constants are intended for use in ISOBUS Virtual Terminal object handling.
- The `OPTIONS`, `JUSTIFICATION`, and `OPTIONS_2` constants encode their settings as bitmasks.
- No runtime behavior, algorithm, or state logic is included.

## State Overview

AID_IN has no state machine and no runtime state. It is a purely declarative element. All values are fixed at compile time and do not change during program execution.

## Application Scenarios

AID_IN is useful in ISOBUS applications that need to configure or update Input Number objects on a Virtual Terminal. Typical scenarios include:

- Creating attribute lists for ISOBUS object updates.
- Reading or writing Input Number object properties.
- Improving code readability by using symbolic names such as `WIDTH` or `VALUE` instead of numeric attribute IDs.
- Supporting maintainable, reusable ISOBUS communication logic in 4diac-ide.

## Comparison with Similar Blocks

AID_IN is not a function block and cannot be directly compared with execution blocks such as controllers or converters. It belongs to a family of constant containers used for different ISOBUS object types. Similar constants may exist for other object attributes, such as output numbers, bar graphs, or text objects. The main difference between such containers is the set of attribute IDs and the meanings associated with them.

## Conclusion

AID_IN provides a clear and maintainable way to handle ISOBUS Input Number object attribute IDs in 4diac-ide. By replacing numeric literals with named constants, it helps avoid errors, improves readability, and supports consistent communication with ISOBUS Virtual Terminals.