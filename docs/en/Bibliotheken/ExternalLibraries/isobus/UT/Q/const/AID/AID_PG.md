# AID_PG

![AID_PG](./AID_PG.svg)

* * * * * * * * * *

## Introduction

The `AID_PG` global constants set defines the attribute identifiers (IDs) for **Picture Graphic (PG)** objects within ISO 11783 (ISOBUS) object pools. These constants are used by function blocks and applications to reference specific properties of picture graphic objects, such as their dimensions, transparency settings, and colour format. By using symbolic names instead of raw numeric values, the code becomes more readable, self-documenting, and resilient against changes in the underlying protocol specifications.

## Interface Structure

`AID_PG` is not a function block or adapter but a collection of global constants. It does not possess dynamic interfaces such as events, data inputs/outputs, or adapters. Instead, it provides a set of named constant values that can be referenced throughout a 4diac project.

### **Event Inputs**

None. The constants set does not process events.

### **Event Outputs**

None. The constants set does not generate events.

### **Data Inputs**

None. The constants set does not accept any data inputs.

### **Data Outputs**

The following global constants are defined, serving as static attribute IDs for picture graphic objects:

| Constant | Type | Initial Value | Description |
|----------|------|---------------|-------------|
| `WIDTH` | `USINT` | `1` | Attribute ID for the width of the picture graphic in pixels (AID_PG_WIDTH). |
| `OPTIONS` | `USINT` | `2` | Attribute ID for an options bitmask – bit 0: transparent, bit 1: flashing, bit 2: run‑length encoded (RLE) (AID_PG_OPTIONS). |
| `TRANSPARENCY_COLOUR` | `USINT` | `3` | Attribute ID for the transparency colour index (AID_PG_TRANSPARENCY_COLOUR). |
| `ACTUAL_WIDTH` | `USINT` | `4` | Attribute ID for the actual width in pixels (AID_PG_ACTUAL_WIDTH). |
| `ACTUAL_HEIGHT` | `USINT` | `5` | Attribute ID for the actual height in pixels (AID_PG_ACTUAL_HEIGHT). |
| `FORMAT` | `USINT` | `6` | Attribute ID for the picture graphic format: 0 = monochrome, 1 = 4‑bit colour, 2 = 8‑bit colour (AID_PG_FORMAT). |

### **Adapters**

None. The constants set does not contain any adapter instances.

## Functionality

The `AID_PG` constants serve as standardised numeric identifiers for the attributes of a picture graphic object in an ISOBUS object pool. When a function block needs to read or write a picture graphic attribute (e.g., during object pool construction, dynamic updates, or diagnostics), it can use these constants as the attribute ID parameter. This approach ensures interoperability with ISOBUS terminals and simplifies the maintenance of object pool definitions.

## Technical Features

- All constants are of type `USINT` (unsigned short integer), requiring only a single byte of storage.
- The values are fixed at compile time and cannot be modified during runtime.
- The constant names follow an intuitive naming convention that corresponds to the official ISOBUS attribute names.
- The constants are defined in a namespace (`isobus::UT::Q::const::AID`) to avoid naming conflicts with other global constants.

## State Overview

Not applicable – a global constants set does not have a state machine. It provides static data that remains unchanged throughout the execution of the application.

## Application Scenarios

The `AID_PG` constants are predominantly used in ISOBUS compatible agricultural machinery and control systems. Typical scenarios include:

- **Object pool generation** – when creating or uploading graphic objects to a Virtual Terminal, the attribute IDs for picture graphics are required to set properties such as width, height, transparency, and format.
- **Dynamic graphic updates** – in applications that modify picture graphics at runtime (e.g., displaying changing weather maps or yield data), these constants allow efficient access to the respective attributes.
- **Diagnostics and debugging** – during development, referencing attribute IDs by symbolic constants enhances code readability and reduces the risk of using incorrect numeric values.

## Comparison with Similar Blocks

In ISOBUS, each object type (e.g., Object, Button, Output, Input, Meter, BarGraph, etc.) has its own set of attribute ID constants. The `AID_PG` set is specifically dedicated to Picture Graphic objects. Comparable constants sets include:

- `AID_OBJ` for general object attributes,
- `AID_BAR` for bar graph attributes,
- `AID_BTN` for button attributes,
- `AID_INP` for input attributes.

Unlike function blocks, these constants do not contain logic or processing capabilities; they merely provide a structured, named collection of numeric IDs that facilitate the implementation of ISOBUS communication.

## Conclusion

The `AID_PG` global constants set provides a clean and standardised way to reference Picture Graphic object attribute IDs in 4diac IDE projects targeting ISOBUS applications. By using these symbolic constants, developers can avoid hard‑coded numeric values, improve code maintainability, and ensure full compliance with the ISO 11783 standard. Although it is not an executable functional element, it plays a crucial supporting role in the development of robust and interoperable agricultural automation systems.
