# AID_IL

![AID_IL](./AID_IL.svg)

* * * * * * * * * *
## Introduction

AID_IL is a global constant group used in 4diac-IDE-based ISOBUS applications. It defines the attribute identifiers for an **Input List** object of an ISOBUS Universal Terminal (UT). The constant group is not a function block or adapter; it provides a named, reusable set of numeric attribute IDs that are used when constructing or manipulating an Input List object in an ISOBUS object pool.

By using the constants defined here, application code avoids “magic numbers” and becomes more readable and maintainable.

## Interface Structure

AID_IL is a global constant list and therefore does not expose an IEC 61499 function block interface. It has no event inputs, event outputs, data inputs, data outputs, or adapters. Its interface consists solely of compile-time constant values.

### **Event Inputs**

None. AID_IL is not an executable function block and defines no event inputs.

### **Event Outputs**

None. AID_IL has no event outputs and cannot trigger further processing.

### **Data Inputs**

None. The constants are global values and are not provided through a data input interface.

### **Data Outputs**

None as a block interface. However, the defined constants can be read globally as `AID_IL.<CONSTANT_NAME>`, for example `AID_IL.WIDTH`.

### **Adapters**

None. AID_IL defines no adapter sockets or plugs.

## Functionality

AID_IL centralizes the attribute IDs of an ISOBUS UT **Input List** object. The following constants are defined:

| Constant | Type | Value | Description |
|---|---|---|---|
| `WIDTH` | USINT | 1 | `AID_IL_WIDTH` – Width of the input list in pixels. |
| `HEIGHT` | USINT | 2 | `AID_IL_HEIGHT` – Height of the input list in pixels. |
| `VARIABLE_REF` | USINT | 3 | `AID_IL_VARIABLE_REF` – Object ID of a Number Variable object that is linked to the selected list item. |
| `VALUE` | USINT | 4 | `AID_IL_VALUE` – Selected list index, from 0 to 254, or 255 if no item is selected. |
| `OPTIONS` | USINT | 5 | `AID_IL_OPTIONS` – Bitmask for input list options. Bit 0 controls whether the object is enabled; bit 1 controls real-time editing. |

All constants are declared as `USINT`, an unsigned short integer value in the range 0 to 255. They are defined globally in the constant package:

```text
isobus::UT::Q::const::AID
```

## Technical Features

- Provides five named constants for Input List object attributes.
- All constants use the `USINT` data type.
- The values correspond to the attribute IDs used by the ISOBUS Universal Terminal protocol.
- The constant group can be reused in multiple function blocks without duplicate definitions.
- The included `OPTIONS` bitmask supports two distinct configuration flags in a single attribute.
- Because the constants are compile-time constants, they do not consume runtime resources and cannot be changed accidentally during execution.

## State Overview

AID_IL is not stateful. It defines fixed values that are resolved at compile time. There is no initialization procedure, no internal state machine, and no runtime behavior. The values remain constant for the entire lifetime of the application.

## Application Scenarios

AID_IL is typically used when an ISOBUS UT object pool or a client function block needs to set or query attributes of an Input List object. For example:

- Setting the width and height of an Input List object.
- Linking the Input List to a Number Variable object that stores the selected item.
- Reading or setting the currently selected list index.
- Changing the enabled state or activating real-time editing through the options bitmask.

Using `AID_IL.WIDTH` instead of the literal value `1` makes the application code easier to understand and to migrate if the attribute numbering changes.

## Comparison with Similar Blocks

AID_IL is not a function block, so it cannot be directly compared to blocks that process events or data. Instead, it serves a similar role to an enumeration or a set of preprocessor constants in other programming languages.

Compared to a typical constant function block that outputs a single value, AID_IL provides multiple related constants in one global namespace. Compared to an adapter, it does not define a communication interface. Its main advantage is that all Input List attribute IDs are collected in one place and can be used consistently across many applications.

## Conclusion

AID_IL is a small but useful global constant group for ISOBUS Universal Terminal applications. It defines the attribute IDs for an Input List object and helps keep application code readable, consistent, and independent from hard-coded numeric IDs. Although it has no executable interface, it is a valuable library element in 4diac-IDE-based ISOBUS projects.