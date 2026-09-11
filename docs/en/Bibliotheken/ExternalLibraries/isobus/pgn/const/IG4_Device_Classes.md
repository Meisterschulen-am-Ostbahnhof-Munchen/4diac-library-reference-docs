# IG4_Device_Classes

![IG4_Device_Classes](./IG4_Device_Classes.svg)

* * * * * * * * * *

## Introduction

`IG4_Device_Classes` is a global constant container for Industry Group 4 specific device classes, also referred to as vehicle systems. It defines a set of named `BYTE` constants that represent standardized vehicle system categories. These constants are intended for use in ISOBUS/J1939-related applications and PGN processing.

The constant names use the prefix `DC_`, meaning Device Class, and provide readable symbols for numeric codes used to identify vehicle systems.

* * * * * * * * * *

## Interface Structure

`IG4_Device_Classes` is not an executable function block, adapter, or subapplication. It does not have an IEC 61499 event/data interface. The following interface categories are therefore not applicable.

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

* * * * * * * * * *

## Functionality

The main function of `IG4_Device_Classes` is to provide a single source of truth for device class codes related to Industry Group 4 vehicle systems. Instead of using magic numbers in function blocks or ST/CFC applications, a developer can reference the named constants directly.

These constants can be used when constructing or decoding ISOBUS/J1939 device names, vehicle system identifiers, or any protocol data field that uses a `BYTE` value to represent a device class.

* * * * * * * * * *

## Technical Features

- All constants are defined as `BYTE` values.
- Values are fixed and read-only.
- Constant names use the `DC_` prefix for consistent identification.
- The constants belong to the compiler package `isobus::pgn::const`.
- The set covers the standard device class values for Industry Group 4 vehicle systems.

| Constant | Value | Description |
|---|---|---:|---|
| `DC_NON_SPECIFIC_SYSTEM` | 0 | Non-specific System |
| `DC_SYSTEM_TOOLS` | 10 | System tools |
| `DC_SAFETY_SYSTEMS` | 20 | Safety systems |
| `DC_GATEWAY` | 25 | Gateway |
| `DC_POWER_MGMT_LIGHTING` | 30 | Power management and lighting systems |
| `DC_STEERING_SYSTEMS` | 40 | Steering systems |
| `DC_PROPULSION_SYSTEMS` | 50 | Propulsion systems |
| `DC_NAVIGATION_SYSTEMS` | 60 | Navigation systems |
| `DC_COMMUNICATIONS_SYSTEMS` | 70 | Communications systems |
| `DC_INSTRUMENTATION_GENERAL` | 80 | Instrumentation/general systems |
| `DC_ENVIRONMENTAL_HVAC` | 90 | Environmental (HVAC) systems |
| `DC_DECK_CARGO_FISHING` | 100 | Deck, cargo, and fishing equipment systems |
| `DC_NOT_AVAILABLE` | 127 | Not Available |

* * * * * * * * * *

## State Overview

Not applicable. `IG4_Device_Classes` is a global constant container and does not define an execution state machine. No states, transitions, or event-driven behavior are associated with this element.

* * * * * * * * * *

## Application Scenarios

Typical application areas include:

- ISOBUS/J1939 device name construction.
- Identification of vehicle systems in electronic control units.
- Filtering or routing of messages based on device class.
- Configuration of agricultural or vehicle systems.
- Replacement of numeric constants with readable symbolic names in ST and CFC code.

* * * * * * * * * *

## Comparison with Similar Blocks

`IG4_Device_Classes` is not a function block and therefore does not perform calculations or process data flows. In this respect, it is similar to a global variable list or a constant table.

Compared with executable function blocks, it offers the following benefits:

- Fixed, non-modifiable values.
- Global availability across a project.
- Clear symbolic naming.
- Direct support for protocol-specific device class values.

It can be compared with other industry-group-specific constant sets, where the same naming convention is used but the assigned values and device class definitions may differ.

* * * * * * * * * *

## Conclusion

`IG4_Device_Classes` provides a clear and maintainable set of `BYTE` constants for Industry Group 4 vehicle device classes. It removes ambiguity from numeric values and supports ISOBUS/J1939 application development by offering meaningful symbolic names. The element is simple, static, and suitable for use wherever device class information must be referenced consistently.
