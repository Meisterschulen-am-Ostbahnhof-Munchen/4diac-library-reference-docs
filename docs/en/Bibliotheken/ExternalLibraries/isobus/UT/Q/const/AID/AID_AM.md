# AID_AM

![AID_AM](./AID_AM.svg)

* * * * * * * * * *

## Introduction

The **AID_AM** global constant block defines attribute identifiers used within the ISO 11783 (ISOBUS) application layer for **Alarm Mask objects**. These constants provide standardized numeric IDs that allow applications to read or set specific attributes of an alarm mask, such as background colour, associated soft key mask, alarm priority, and acoustic signal configuration. The block is part of a package named `isobus::UT::Q::const::AID` and is intended to be used in 4diac‑IDE projects that interact with ISOBUS terminals or implement virtual terminals.

## Interface Structure

Since `AID_AM` is a **global constants** block, it does not contain event or data input/output ports. Instead, it exposes a set of compile-time constant values that can be referenced globally within the application.

### **Data Constants**

| Constant Name      | Type    | Initial Value | Description |
|--------------------|---------|---------------|-------------|
| `BACKGROUND_COLOUR`| `USINT` | `1`           | Attribute ID for the background colour index of an Alarm Mask. |
| `SOFT_KEY_MASK`    | `USINT` | `2`           | Attribute ID for the object ID of a Soft Key Mask associated with the Alarm Mask. |
| `ALARM_PRIORITY`   | `USINT` | `3`           | Attribute ID for the alarm priority (0 = High, 1 = Medium, 2 = Low). |
| `ACOUSTIC`         | `USINT` | `4`           | Attribute ID for the acoustic signal setting (0 = highest priority, 1 = medium, 2 = lowest, 3 = silent). |

*Note: The constant values themselves are the attribute IDs that are used when communicating with an ISOBUS object pool or handling alarm mask properties.*

## Functionality

The primary purpose of `AID_AM` is to provide a human-readable and self-documenting set of constants that map to the numeric attribute IDs defined in the ISOBUS standard for alarm masks. Instead of hard-coding numeric values directly in function blocks or algorithms, developers can reference these constants to improve code clarity and maintainability. For example, to set the background colour of an alarm mask, one would use `AID_AM.BACKGROUND_COLOUR` as the attribute ID in a corresponding service call.

## Technical Features

- **Standard Compliance**: The constants are aligned with ISO 11783-6 attribute definitions for Alarm Mask objects.
- **Type Safety**: All constants are declared as `USINT` (unsigned 8-bit integer), matching the expected attribute ID range (0–255).
- **Global Scope**: Being `GLOBAL CONSTANT`, they are accessible from any function block or resource in the project without explicit import.
- **Documentation**: Each constant includes a descriptive comment explaining its meaning and possible values.

## State Overview

As a constants block, `AID_AM` has no internal state, states, or state transitions. It represents a static set of values that are available throughout the execution of the application.

## Application Scenarios

- **Virtual Terminal Integration**: When implementing an ISOBUS virtual terminal or client, these constants are used to build or modify alarm mask attribute messages (e.g., Set Attribute, Get Attribute).
- **Alarm Handling**: In an agricultural machine control system, the constants help manage alarm priorities, acoustic feedback, and visual appearance of alarms on the terminal.
- **Object Pool Creation**: Developers creating an ISOBUS object pool for a terminal can use these IDs to correctly encode attribute references.

## Comparison with Similar Blocks

`AID_AM` is one of several attribute-identifier constant blocks defined in the same package (e.g., `AID_WK`, `AID_AR`, etc.). While other blocks define attribute IDs for working sets, soft key masks, or other object types, `AID_AM` specifically targets alarm masks. Its simplicity lies in the absence of any logic or interface; it purely provides constants. Unlike a complex function block, it cannot fail, execute, or be reused in a state machine; it exists only to be referenced.

## Conclusion

The `AID_AM` global constants block is a small but essential utility for ISOBUS applications dealing with alarm masks. By centralizing the attribute IDs, it reduces the risk of typing errors, improves code readability, and ensures consistency across the project. It is a perfect example of using compile-time constants to bring clarity to a standards-based communication protocol.
