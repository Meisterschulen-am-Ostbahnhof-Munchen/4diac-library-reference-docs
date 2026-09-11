# IG3_Source_Addresses

![IG3_Source_Addresses](./IG3_Source_Addresses.svg)

* * * * * * * * * *

## Introduction

IG3_Source_Addresses is a global constant definition block used within IEC 61499-based applications to provide predefined ISO 11783 (ISOBUS) source addresses for Industry Group 3. This block defines a set of fixed BYTE values that identify specific device types and reserved address ranges on the ISOBUS network. By centralizing these constants, the block ensures consistent addressing across different function blocks and systems that require ISO 11783 compliant communication.

## Interface Structure

This entity is a global constants definition rather than a typical function block. Therefore, it does not expose any event or data inputs/outputs, nor does it include adapters. Instead, it provides a collection of named constants that can be referenced throughout a project.

### **Event Inputs**

None. This block does not have any event inputs.

### **Event Outputs**

None. This block does not have any event outputs.

### **Data Inputs**

None. This block does not accept any runtime data inputs.

### **Data Outputs**

None in the traditional sense. However, the defined constants act as read-only values that can be read from other blocks. The following table lists all constants defined in this block:

| Constant Name | Type | Value | Description |
| --- | --- | --- | --- |
| SA_RESERVED_128 | BYTE | 128 | Source Address: thru 207 are reserved for future assignment by SAE - Used for dynamic address assignment (self-configurable) |
| SA_RESERVED_208 | BYTE | 208 | Source Address: thru 223 are reserved for future assignment - Used for individual preassigned addresses |
| SA_ROTATION_SENSOR | BYTE | 224 | Source Address: Rotation Sensor - A device that measures the rotational angle around an axis. |
| SA_LIFT_ARM_CONTROLLER | BYTE | 225 | Source Address: Lift Arm Controller - Controls the lift arms and tilt functions on a construction loader, skid steer loader, or similar machine. |
| SA_SLOPE_SENSOR | BYTE | 226 | Source Address: Slope Sensor - A device that measures the slope along an axis. |
| SA_MAIN_CONTROLLER_SKID_STEER_LOADER | BYTE | 227 | Source Address: Main Controller - Skid Steer Loader - Primary system controller for skid steer loader |
| SA_LOADER_CONTROL | BYTE | 228 | Source Address: Loader Control - Controls the hydraulic system of the loader attachment of a loader/backhoe, wheel loader, skid steer, or similar vehicle |
| SA_LASER_TRACER | BYTE | 229 | Source Address: Laser Tracer - A device that receives a laser strike and reports the vertical and horizontal position. |
| SA_LAND_LEVELING_SYSTEM_DISPLAY | BYTE | 230 | Source Address: Land Leveling System Display - This device displays position information at a remote location. |
| SA_SINGLE_LAND_LEVELING_SYSTEM_SUPERVISOR | BYTE | 231 | Source Address: Single Land Leveling System Supervisor - This device is the Land Leveling System Supervisor for a single control loop. |
| SA_LAND_LEVELING_ELECTRIC_MAST | BYTE | 232 | Source Address: Land Leveling Electric Mast - A device that moves a Sensor to maintain a specific position. |
| SA_SINGLE_LAND_LEVELING_SYSTEM_OPERATOR_INTERFACE | BYTE | 233 | Source Address: Single Land Leveling System Operator Interface - A component that allows the user to control the Land Leveling System and display information about the operation of the system. |
| SA_LASER_RECEIVER | BYTE | 234 | Source Address: Laser Receiver - A device that receives a laser strike, and reports the specific position. |
| SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_1 | BYTE | 235 | Source Address: Supplemental Sensor Processing Unit #1 |
| SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_2 | BYTE | 236 | Source Address: Supplemental Sensor Processing Unit #2 |
| SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_3 | BYTE | 237 | Source Address: Supplemental Sensor Processing Unit #3 |
| SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_4 | BYTE | 238 | Source Address: Supplemental Sensor Processing Unit #4 |
| SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_5 | BYTE | 239 | Source Address: Supplemental Sensor Processing Unit #5 |
| SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_6 | BYTE | 240 | Source Address: Supplemental Sensor Processing Unit #6 |
| SA_ENGINE_MONITOR_1 | BYTE | 241 | Source Address: Engine Monitor #1 |
| SA_ENGINE_MONITOR_2 | BYTE | 242 | Source Address: Engine Monitor #2 |
| SA_ENGINE_MONITOR_3 | BYTE | 243 | Source Address: Engine Monitor #3 |
| SA_ENGINE_MONITOR_4 | BYTE | 244 | Source Address: Engine Monitor #4 |
| SA_ENGINE_MONITOR_5 | BYTE | 245 | Source Address: Engine Monitor #5 |
| SA_ENGINE_MONITOR_6 | BYTE | 246 | Source Address: Engine Monitor #6 |
| SA_ENGINE_MONITOR_7 | BYTE | 247 | Source Address: Engine Monitor #7 |

### **Adapters**

None. This block does not contain any adapters.

## Functionality

The IG3_Source_Addresses block provides a centralized and standardized set of source addresses for devices operating in Industry Group 3 of the ISO 11783 (ISOBUS) protocol. Source addresses are essential for uniquely identifying devices on the CAN-based network. By defining them as named constants, the block removes ambiguity in application logic and simplifies maintenance. Devices such as sensors, controllers, and monitors can reference these constants directly, ensuring that the correct address is used without manual hard-coding.

## Technical Features

- All constants are of type BYTE, ranging in value from 128 to 247.
- Values 128 and 208 represent reserved address ranges as defined by the SAE and are used for dynamic address assignment and individual preassigned addresses, respectively.
- Values 224 through 234 are assigned to specific device functions such as rotation sensors, lift arm controllers, slope sensors, loaders, laser tracers, land leveling equipment, and laser receivers.
- Values 235 through 240 are reserved for supplemental sensor processing units.
- Values 241 through 247 are assigned to engine monitors (up to 7 monitors).
- The block is defined as a global constant container, making the constants visible to all function blocks within the project without duplication.

## State Overview

This block does not have an internal state machine or execute any runtime behavior. It is a static definition entity. The constants it defines are immutable and do not change during the execution of an application.

## Application Scenarios

- **ISOBUS Implement Control:** When building a control system for agricultural or construction machinery that uses ISOBUS, these constants can be used to assign source addresses to the various electronic control units (ECUs).
- **Sensor Integration:** Connecting rotation sensors, slope sensors, or laser receivers to an ISOBUS network, ensuring each device uses the correct predefined address.
- **Engine Monitoring:** Using the engine monitor address constants to identify up to seven engine monitoring devices on a machine.
- **Reserved Address Handling:** Applications that need to dynamically assign addresses can use the reserved address constants (SA_RESERVED_128 and SA_RESERVED_208) as starting points for self-configuration or preassigned address management.

## Comparison with Similar Blocks

In contrast to IEC 61499 function blocks that process data via event and data connections, this block is purely declarative. It serves a similar purpose to other global constant containers, such as `IG1_Source_Addresses` or `IG2_Source_Addresses`, which define source addresses for Industry Groups 1 and 2. The main difference lies in the specific device types and address ranges assigned to each industry group. Compared to an ordinary function block, this definition offers no executable logic but provides a clear, reusable reference that enhances maintainability and reduces errors in address allocation.

## Conclusion

The IG3_Source_Addresses global constant block provides a well-organized collection of ISO 11783 source addresses for Industry Group 3 devices. It simplifies the development of ISOBUS-compliant applications by offering named, typed constants that are easy to reference and manage. This approach promotes code clarity, consistency, and correct network addressing, making it a valuable building block for machinery control and telemetry systems.
