# IG1_NAME_Functions

![IG1_NAME_Functions](./IG1_NAME_Functions.svg)

* * * * * * * * * *

## Introduction

The `IG1_NAME_Functions` global constant definition belongs to the ISO 11783 (ISOBUS) protocol stack and provides a standardized set of byte-valued constants for identifying **Industry Group 1 (IG1)** specific function types within a vehicle's electronic control unit (ECU) network. This constant set is used to populate the Function field of the ISO 11783 NAME parameter, enabling unambiguous identification of the role each ECU plays on the bus.

The constants are organized according to the three device classes within Industry Group 1: non-specific systems, tractors, and trailers. Each constant maps a semantic function name to a numeric code that can be transmitted and interpreted across the bus network.

## Interface Structure

> **Note:** This artifact is a global constants definition, not a function block, adapter, or subapplication. Therefore, it does not expose event or data ports. The "interface" represents the set of named constant values that can be referenced globally by IEC 61499 applications.

### **Event Inputs**

None. The `IG1_NAME_Functions` constant group defines data values only and does not provide any event inputs.

### **Event Outputs**

None. There are no event outputs defined in this constant group.

### **Data Inputs**

None. The constants are static and read-only at runtime; they cannot be modified via data inputs.

### **Data Outputs**

The following global constants are made available to IEC 61499 applications for reference:

| System Group | Constant Name | Type | Value | Description |
|---|---|---|---|---|
| Non-specific System | `F_TACHOGRAPH` | BYTE | 128 | Non-specific system, function: Tachograph |
| Non-specific System | `F_DOOR_CONTROLLER` | BYTE | 129 | Non-specific system, function: Door Controller |
| Non-specific System | `F_ARTICULATION_TURNTABLE_CONTROL` | BYTE | 130 | Non-specific system, function: Articulation Turntable Control – controls the articulation turntable for joined-body buses |
| Non-specific System | `F_BODY_TO_VEHICLE_INTERFACE_CONTROL` | BYTE | 131 | Non-specific system, function: Body-to-Vehicle Interface Control – manages interaction between vehicle functions and body functions |
| Non-specific System | `F_SLOPE_SENSOR` | BYTE | 132 | Non-specific system, function: Slope Sensor – measures a slope along an axis |
| Non-specific System | `F_RETARDER_DISPLAY` | BYTE | 134 | Non-specific system, function: Retarder Display – shows retarder information (driveline, exhaust, or engine) |
| Non-specific System | `F_DIFFERENTIAL_LOCK_CONTROLLER` | BYTE | 135 | Non-specific system, function: Differential Lock Controller |
| Non-specific System | `F_LOW_VOLTAGE_DISCONNECT` | BYTE | 136 | Non-specific system, function: Low-Voltage Disconnect – monitors starting battery voltage and disconnects auxiliary loads |
| Non-specific System | `F_ROADWAY_INFORMATION` | BYTE | 137 | Non-specific system, function: Roadway Information – provides information about the roadway in which the vehicle is traveling |
| Non-specific System | `F_AUTOMATED_DRIVING` | BYTE | 138 | Non-specific system, function: Automated Driving – hardware and software capable of performing part or all of the dynamic driving task |
| Non-specific System | `F_NOT_AVAILABLE` | BYTE | 255 | Non-specific system, function: Not Available – placeholder until an explicit function is assigned |
| Tractor | `F_TRACTOR_FORWARD_ROAD_IMAGE_PROCESSING` | BYTE | 128 | Tractor, function: Forward Road Image Processing – determines vehicle position from lane markings (advisory/warning only) |
| Tractor | `F_TRACTOR_FIFTH_WHEEL_SMART_SYSTEM` | BYTE | 129 | Tractor, function: Fifth Wheel Smart System – systems related to fifth wheel coupler operation and safety monitoring |
| Tractor | `F_TRACTOR_CATALYST_FLUID_SENSOR` | BYTE | 130 | Tractor, function: Catalyst Fluid Sensor – measures catalyst fluid temperature, level, and quality |
| Tractor | `F_TRACTOR_ADAPTIVE_FRONT_LIGHTING_SYSTEM` | BYTE | 131 | Tractor, function: Adaptive Front Lighting System – adjusts front lighting for current operating conditions |
| Tractor | `F_TRACTOR_IDLE_CONTROL_SYSTEM` | BYTE | 132 | Tractor, function: Idle Control System – automatically starts and stops the engine to reduce excess idle time |
| Tractor | `F_TRACTOR_USER_INTERFACE_SYSTEM` | BYTE | 133 | Tractor, function: User Interface System – two-way interface for climate, system parameters, and other settings |
| Tractor | `F_TRACTOR_NOT_AVAILABLE` | BYTE | 255 | Tractor, function: Not Available – placeholder until an explicit function is assigned |
| Trailer | `F_TRAILER_NOT_AVAILABLE` | BYTE | 255 | Trailer, function: Not Available – placeholder until an explicit function is assigned |

### **Adapters**

None. The constant group does not use or provide adapter interfaces.

## Functionality

The `IG1_NAME_Functions` constant set serves as a lookup table for ISO 11783 NAME function codes within Industry Group 1. When composing an ECU NAME message, an application selects the appropriate constant based on the device's role (e.g., a tachograph, a door controller, or a slope sensor). The selected byte value is then encoded into the NAME field and broadcast on the bus, allowing other ECUs to determine the capability and purpose of the sending device.

The naming convention follows the pattern `F_<SYSTEM>_<FUNCTION>` where the system prefix is omitted for non-specific system functions, and the value space is organized so that each device class (non-specific, tractor, trailer) has its own starting code range. The value `255` is reserved in every system group to indicate that no specific function has been assigned yet.

## Technical Features

- **Data Type:** All constants are declared as `BYTE` (8-bit unsigned) to fit the ISO 11783 NAME function field.
- **Value Ranges:** Within each system group, valid function codes start at `128` and increase sequentially. The value `255` is universally used as the "Not Available" placeholder.
- **Grouping:** Constants are logically grouped by device class – non-specific system, tractor, and trailer – following the `DC_*` (Device Class) definitions of the ISO 11783 standard.
- **Global Scope:** Being declared as `VAR_GLOBAL CONSTANT`, the constants are available throughout an IEC 61499 application without additional instantiation.
- **Compiler Hint:** The bundled compiler information (`isobus::pgn::const`) indicates that the constants are intended to be used within the ISOBUS PGN (Parameter Group Number) namespace, aiding tool-based integration.

## State Overview

The `IG1_NAME_Functions` constant group is entirely static and stateless. There is no runtime state machine associated with it. The values are fixed at compile time and do not change during execution. Consequently, there are no states, transitions, or lifecycle operations to describe.

## Application Scenarios

- **ECU Identification:** A control application that needs to send its NAME to the ISOBUS network can reference `F_SLOPE_SENSOR` (for a slope sensor) or `F_TRACTOR_FIFTH_WHEEL_SMART_SYSTEM` (for a tractor's fifth wheel system) to correctly advertise its function.
- **Function Assignment:** During system commissioning, a device may be programmed with `F_NOT_AVAILABLE` (255) until its final function is determined, ensuring it is recognized as placeholder equipment on the bus.
- **Multi-Function Devices:** A body controller that manages both doors and articulation turntable controls can use the corresponding constants to indicate multiple capabilities (if the underlying protocol supports multiple NAME entries).
- **Diagnostics and Monitoring:** Service tools can map received NAME function codes back to human-readable names using these constants, simplifying troubleshooting.

## Comparison with Similar Blocks

The `IG1_NAME_Functions` constant group is specific to **Industry Group 1** of the ISO 11783 standard. Similar constant sets exist for other industry groups (e.g., IG0 – agriculture-specific, IG2 – forestry), each defining their own function codes. The key difference is that IG1 covers general freight and passenger vehicle applications (tractors, trailers, and non-specific systems) and therefore includes function codes such as tachograph, door controller, and automated driving which are not present in other groups.

In contrast to a typical IEC 61499 function block, which contains event and data interfaces along with an execution algorithm, this constant group has no executable behavior. It is a pure data artifact that serves as a shared vocabulary for function naming. It may be complemented by a separate adapter or function block that performs the actual encoding/decoding of NAME messages, but the constant group itself remains passive.

## Conclusion

The `IG1_NAME_Functions` global constants provide a standardized, easy-to-reference mapping of ISO 11783 Industry Group 1 function codes. By centralizing the function definitions in a single location, it reduces the risk of value collisions and improves code readability across ISOBUS-related IEC 61499 applications. The constants are applicable to a wide range of vehicle control scenarios, from simple trailer monitoring to complex automated driving systems, and are an essential building block for any compliant ISOBUS implementation targeting agricultural, construction, or commercial vehicles.
