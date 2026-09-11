# AID_WM

![AID_WM](./AID_WM.svg)

* * * * * * * * * *
## Introduction

The `AID_WM` global constant group defines attribute identifiers for window mask objects within the ISOBUS (ISO 11783) Universal Terminal protocol. These constants are used as keys to reference specific attributes when reading or writing window mask object properties through the ISOBUS communication interface. They belong to the `isobus::UT::Q::const::AID` package and are defined in the namespace of the 4diac-ide development environment.

## Interface Structure

Since `AID_WM` is defined as a global constants group rather than a function block, it does not expose event inputs, event outputs, data inputs, data outputs, or adapter interfaces. Instead, it provides a set of named constant values that can be referenced throughout the application. The following constant values are available within this group:

### Data Constants

| Constant Name     | Data Type | Initial Value | Description |
|-------------------|-----------|---------------|-------------|
| `BACKGROUND_COLOUR` | USINT     | `USINT#1`     | Attribute ID 1: Window mask background colour index. |
| `OPTIONS`          | USINT     | `USINT#2`     | Attribute ID 2: Bitmask for window mask options. |
| `NAME`             | USINT     | `USINT#3`     | Attribute ID 3: Object ID of an Output String or Object Pointer containing the window mask name. |

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

The `AID_WM` constants serve as identifiers matching the attribute IDs defined by the ISOBUS standard for window mask objects. When interacting with a window mask object on an ISOBUS Universal Terminal, these IDs are used in command and response messages to denote which specific attribute of the object is being addressed.

- **`BACKGROUND_COLOUR`** refers to the colour index used to render the background of the window mask. The colour index is typically interpreted according to the terminal's colour palette.
- **`OPTIONS`** is a bitmask that encodes configuration flags for the window mask: Bit 0 indicates availability (when set to 0, the window is disabled or blanked), and Bit 1 indicates transparency (when set, the window does not use a background colour).
- **`NAME`** provides a reference to another object (an Output String or Object Pointer) that contains the display name of the window mask.

By using these predefined constants, applications avoid hardcoded numeric values and improve code readability and maintainability.

## Technical Features

- **Data Type**: All constants are of type `USINT` (Unsigned Short Integer, 8 bits).
- **Initial Values**: Each constant is initialized with its corresponding attribute ID value.
- **Namespace**: The constants reside in the `isobus::UT::Q::const::AID` package, which organizes attribute ID constants systematically for ISOBUS object types.
- **Licensing**: The original source is provided under the Eclipse Public License 2.0 (EPL-2.0).
- **Compiler Compatibility**: The package is intended for use with the 4diac-ide compiler environment targeting IEC 61499 applications.

## State Overview

The `AID_WM` constant group does not maintain any runtime state. It is a compile-time constant definition, and all values are statically initialized. No state transitions or lifecycle management apply.

## Application Scenarios

The `AID_WM` constants are used in agricultural machinery and implement control systems where a Universal Terminal (UT) communicates with an implement ECU (Electronic Control Unit) via the ISOBUS network. Typical application scenarios include:

- **Window Mask Creation**: When defining a window mask object on the UT, the application uses `AID_WM` constants to specify which attribute to set during initialization.
- **Attribute Retrieval**: When requesting the current value of a window mask attribute (e.g., background colour), the constant IDs are inserted into the command message.
- **Dynamic Configuration**: The `OPTIONS` constant allows runtime toggling of the window's availability or transparency, for instance, to disable a menu entry when a function is not operational.
- **UI Text Management**: The `NAME` constant is used to associate a text object with a window mask, enabling dynamic label changes.

## Comparison with Similar Blocks

Within the same ISOBUS attribute ID constant family, similar groups exist for other object types, such as:

- **`AID_BTC`** – Button and ButtonMask attribute IDs.
- **`AID_OS`** – Output String attribute IDs.
- **`AID_ALM`** – Alarm object attribute IDs.
- **`AID_EP`** – External Input/Output Point attribute IDs.

While all these groups follow the same pattern of defining USINT constants for attribute IDs, `AID_WM` specifically targets window mask objects. Unlike a function block, which provides executable logic and event-driven behavior, `AID_WM` is purely a data declaration that supplies numeric identifiers. This makes it similar to enumerations or `#define` constants in traditional programming, but with the advantage of being a strongly typed global constant within the 4diac-ide environment.

## Conclusion

The `AID_WM` global constants provide a standardized, readable way to reference window mask object attributes in ISOBUS applications. By encapsulating the attribute IDs as named constants, the group enhances code maintainability and reduces the risk of errors from magic numbers. As part of the broader `isobus::UT::Q::const::AID` package, it integrates seamlessly with other attribute ID constants, supporting a coherent development approach for ISO 11783-compliant systems in 4diac-ide.