# AID_GC

![AID_GC](./AID_GC.svg)

* * * * * * * * * *

## Introduction

The `AID_GC` definition provides a standardized set of global constants used to identify attribute IDs for Graphic Context (GC) objects in ISOBUS-based agricultural systems (ISO 11783-6). These constants simplify referencing common graphical parameters—such as viewport dimensions, colours, and formatting—within control applications and human-machine interfaces. The values are defined as unsigned short integers (`USINT`) and are intended to be used as attribute identifiers when configuring graphic context objects.

## Interface Structure

### **Event Inputs**

None – this is not a function block with event interfaces. It constitutes a set of global constant declarations.

### **Event Outputs**

None – no event outputs are defined.

### **Data Inputs**

None – the definition does not provide any data input ports. All values are statically defined constants.

### **Data Outputs**

None – no data output ports are present.

### **Adapters**

None – no adapters are associated with this definition.

## Functionality

The primary purpose of `AID_GC` is to offer a readable and consistent way to refer to attribute IDs of ISOBUS graphic context objects. Each named constant corresponds to a numeric attribute ID, enabling developers to avoid hard-coded numbers in their code. The constants cover:

- Viewport properties (width, height, position, zoom)
- Canvas dimensions
- Cursor coordinates
- Colour settings (foreground, background, transparency)
- Referencing font, line, and fill attribute objects
- Format and option flags

The following table lists all defined constants:

| Constant Name          | Type  | Initial Value | Description                                                                                   |
|------------------------|-------|---------------|-----------------------------------------------------------------------------------------------|
| `VP_WIDTH`             | USINT | `USINT#1`      | Viewport width attribute ID                                                                  |
| `VP_HEIGHT`            | USINT | `USINT#2`      | Viewport height attribute ID                                                                 |
| `VP_X`                 | USINT | `USINT#3`      | Viewport X coordinate attribute ID                                                            |
| `VP_Y`                 | USINT | `USINT#4`      | Viewport Y coordinate attribute ID                                                            |
| `CANVAS_WIDTH`         | USINT | `USINT#5`      | Canvas width attribute ID                                                                    |
| `CANVAS_HEIGHT`        | USINT | `USINT#6`      | Canvas height attribute ID                                                                   |
| `VP_ZOOM`              | USINT | `USINT#7`      | Viewport zoom attribute ID                                                                   |
| `GR_CURSOR_X`          | USINT | `USINT#8`      | Graphic cursor X coordinate attribute ID                                                     |
| `GR_CURSOR_Y`          | USINT | `USINT#9`      | Graphic cursor Y coordinate attribute ID                                                     |
| `FG_COLOUR`            | USINT | `USINT#10`     | Foreground colour index attribute ID                                                         |
| `BACKGROUND_COLOUR`    | USINT | `USINT#11`     | Background colour index attribute ID                                                         |
| `FONT_ATT`             | USINT | `USINT#12`     | Object ID reference for a Font Attributes object                                             |
| `LINE_ATT`             | USINT | `USINT#13`     | Object ID reference for a Line Attributes object                                             |
| `FILL_ATT`             | USINT | `USINT#14`     | Object ID reference for a Fill Attributes object                                             |
| `FORMAT`               | USINT | `USINT#15`     | Canvas type: 0 = Monochrome, 1 = 4-bit colour, 2 = 8-bit colour                              |
| `OPTIONS`              | USINT | `USINT#16`     | Bitmask: Bit 0 = Transparency (0 = opaque, 1 = transparent), Bit 1 = Colour mode             |
| `TRANS_COLOUR`         | USINT | `USINT#17`     | Transparency colour attribute ID                                                             |

## Technical Features

- **Data Type**: All constants are of type `USINT` (unsigned short integer).
- **Initialization**: Each constant is initialized with a distinct numeric value, directly mapping to the attribute ID it represents.
- **Package Association**: The constants are defined in the package `isobus::UT::Q::const::AID`, providing a clear namespace for ISOBUS constant definitions.
- **Standards Compliance**: The definitions follow the IEC 61499-1 standard for global constants, as indicated in the identification section.
- **Documentation**: Each constant is accompanied by a comment that includes its numeric ID and, where applicable, a description of its purpose (e.g., `BACKGROUND_COLOUR` indicates it stores a colour index; `FORMAT` explains the canvas colour depth).

## State Overview

This definition does not introduce any stateful behavior. The constants are immutable and purely declarative. No state transitions, timers, or conditional logic are present.

## Application Scenarios

- **Graphic Context Configuration**: In ISOBUS terminals, these constants are used to set or query attributes of graphic context objects, such as defining the size of the drawing area or selecting colour modes.
- **HMI Development**: Developers can reference these constants to avoid magic numbers when programming user interface elements, improving code readability and maintainability.
- **Cross-Platform Consistency**: By using standardized attribute IDs, applications can ensure consistent behavior across different ISOBUS implementations.
- **Protocol Integration**: The constants align with the ISOBUS protocol's attribute numbering, simplifying integration with low-level communication layers.

## Comparison with Similar Blocks

While no direct function blocks are compared here, `AID_GC` can be considered analogous to other constant sets used in ISOBUS, such as attribute IDs for object pools (`AID_OP`) or working sets (`AID_WS`). In comparison:

- **Readability**: Named constants provide a clearer alternative to raw numeric IDs.
- **Maintainability**: Centralizing attribute IDs in one location reduces the risk of errors when the protocol evolves.
- **Portability**: Following the official ISOBUS numbering ensures that code is interoperable across compliant devices.

## Conclusion

`AID_GC` is a vital set of global constants for ISOBUS graphic context handling. It offers a structured, documented, and standardized way to reference critical graphical attributes, reducing development effort and enhancing reliability in agricultural automation systems. Its integration into the package `isobus::UT::Q::const::AID` underscores its role as a foundational building block for ISOBUS-based applications.
