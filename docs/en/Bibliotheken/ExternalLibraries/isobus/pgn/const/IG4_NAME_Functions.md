# IG4_NAME_Functions

![IG4_NAME_Functions](./IG4_NAME_Functions.svg)

* * * * * * * * * *

## Introduction

This global constant list defines the Industry Group 4 (IG4) specific function codes used in ISO 11783 (ISOBUS) networks. It provides a standardized mapping between numeric function identifiers and their meanings within various system categories, such as propulsion, navigation, communications, instrumentation, and more. The constants enable consistent identification of ECU functions across the network.

## Interface Structure

Since this is a global constant list rather than a function block, it does not possess a traditional interface with events or adapters. The constants are globally accessible and can be referenced in any function block, subapplication, or script within the project.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The constants defined in this list serve as global data outputs. They are all of type `BYTE` and carry predefined values that represent specific functions. The constants are grouped by system category, as summarized in the table below:

| Constant Name | Value | System | Function Description |
|---------------|-------|--------|----------------------|
| F_ALARM_SYSTEM_CONTROL_FOR_MARINE_ENGINES | 128 | Non-specific System | Alarm System Control for Marine Engines |
| F_PROTECTION_SYSTEM_FOR_MARINE_ENGINES | 129 | Non-specific System | Protection System for Marine Engines |
| F_DISPLAY_FOR_PROTECTION_SYSTEM_FOR_MARINE_ENGINES | 130 | Non-specific System | Display for Protection System for Marine Engines |
| F_NOT_AVAILABLE | 255 | Non-specific System | Not Available |
| F_SYSTEM_TOOLS_NOT_AVAILABLE | 255 | System tools | Not Available |
| F_SAFETY_SYSTEMS_NOT_AVAILABLE | 255 | Safety systems | Not Available |
| F_GATEWAY | 130 | Gateway | Gateway |
| F_POWER_MGMT_LIGHTING_SWITCH | 130 | Power management and lighting systems | Switch |
| F_POWER_MGMT_LIGHTING_LOAD | 140 | Power management and lighting systems | Load |
| F_STEERING_SYSTEMS_FOLLOW_UP_CONTROLLER | 130 | Steering systems | Follow-up Controller |
| F_STEERING_SYSTEMS_MODE_CONTROLLER | 140 | Steering systems | Mode Controller |
| F_STEERING_SYSTEMS_AUTOMATIC_STEERING_CONTROLLER | 150 | Steering systems | Automatic Steering Controller |
| F_STEERING_SYSTEMS_HEADING_SENSORS | 160 | Steering systems | Heading Sensors |
| F_PROPULSION_SYSTEMS_ENGINEROOM_MONITORING | 130 | Propulsion systems | Engineroom monitoring |
| F_PROPULSION_SYSTEMS_ENGINE_INTERFACE | 140 | Propulsion systems | Engine Interface |
| F_PROPULSION_SYSTEMS_ENGINE_CONTROLLER | 150 | Propulsion systems | Engine Controller |
| F_PROPULSION_SYSTEMS_ENGINE_GATEWAY | 160 | Propulsion systems | Engine Gateway |
| F_PROPULSION_SYSTEMS_CONTROL_HEAD | 170 | Propulsion systems | Control Head |
| F_PROPULSION_SYSTEMS_ACTUATOR | 180 | Propulsion systems | Actuator |
| F_PROPULSION_SYSTEMS_GAUGE_INTERFACE | 190 | Propulsion systems | Gauge Interface |
| F_PROPULSION_SYSTEMS_GAUGE_LARGE | 200 | Propulsion systems | Gauge Large |
| F_PROPULSION_SYSTEMS_GAUGE_SMALL | 210 | Propulsion systems | Gauge Small |
| F_PROPULSION_SYSTEMS_PROPULSION_SENSORS_GATEWAY | 220 | Propulsion systems | Propulsion Sensors & Gateway |
| F_NAVIGATION_SYSTEMS_SOUNDER_DEPTH | 130 | Navigation systems | Sounder, depth |
| F_NAVIGATION_SYSTEMS | 140 | Navigation systems | (no specific function description) |
| F_NAVIGATION_SYSTEMS_GLOBAL_NAVIGATION_SATELLITE_SYSTEM_GNSS | 145 | Navigation systems | Global Navigation Satellite System (GNSS) |
| F_NAVIGATION_SYSTEMS_LORAN_C | 150 | Navigation systems | Loran C |
| F_NAVIGATION_SYSTEMS_SPEED_SENSORS | 155 | Navigation systems | Speed Sensors |
| F_NAVIGATION_SYSTEMS_TURN_RATE_INDICATOR | 160 | Navigation systems | Turn Rate Indicator |
| F_NAVIGATION_SYSTEMS_INTEGRATED_NAVIGATION | 170 | Navigation systems | Integrated Navigation |
| F_NAVIGATION_SYSTEMS_RADAR_AND_OR_RADAR_PLOTTING | 200 | Navigation systems | Radar and/or Radar Plotting |
| F_NAVIGATION_SYSTEMS_ELECTRONIC_CHART_DISPLAY_INFORMATION_SYSTEM_ECDIS | 205 | Navigation systems | Electronic Chart Display & Information System (ECDIS) |
| F_NAVIGATION_SYSTEMS_ELECTRONIC_CHART_SYSTEM_ECS | 210 | Navigation systems | Electronic Chart System (ECS) |
| F_NAVIGATION_SYSTEMS_DIRECTION_FINDER | 220 | Navigation systems | Direction Finder |
| F_COMMUNICATIONS_SYSTEMS_EMERGENCY_POSITION_INDICATING_BEACON_EPIRB | 130 | Communications systems | Emergency Position Indicating Beacon (EPIRB) |
| F_COMMUNICATIONS_SYSTEMS_AUTOMATIC_IDENTIFICATION_SYSTEM | 140 | Communications systems | Automatic Identification System |
| F_COMMUNICATIONS_SYSTEMS_DIGITAL_SELECTIVE_CALLING_DSC | 150 | Communications systems | Digital Selective Calling (DSC) |
| F_COMMUNICATIONS_SYSTEMS_DATA_RECEIVER | 160 | Communications systems | Data Receiver |
| F_COMMUNICATIONS_SYSTEMS_SATELLITE | 170 | Communications systems | Satellite |
| F_COMMUNICATIONS_SYSTEMS_RADIO_TELEPHONE_MF_HF | 180 | Communications systems | Radio-Telephone (MF/HF) |
| F_COMMUNICATIONS_SYSTEMS_RADIO_TELEPHONE_VHF | 190 | Communications systems | Radio-Telephone (VHF) |
| F_INSTRUMENTATION_GENERAL_TIME_DATE_SYSTEMS | 130 | Instrumentation/general systems | Time/Date systems |
| F_INSTRUMENTATION_GENERAL_VOYAGE_DATA_RECORDER | 140 | Instrumentation/general systems | Voyage Data Recorder |
| F_INSTRUMENTATION_GENERAL_INTEGRATED_INSTRUMENTATION | 150 | Instrumentation/general systems | Integrated Instrumentation |
| F_INSTRUMENTATION_GENERAL_GENERAL_PURPOSE_DISPLAYS | 160 | Instrumentation/general systems | General Purpose Displays |
| F_INSTRUMENTATION_GENERAL_GENERAL_SENSOR_BOX | 170 | Instrumentation/general systems | General Sensor Box |
| F_INSTRUMENTATION_GENERAL_WEATHER_INSTRUMENTS | 180 | Instrumentation/general systems | Weather Instruments |
| F_INSTRUMENTATION_GENERAL_TRANSDUCER_GENERAL | 190 | Instrumentation/general systems | Transducer/general |
| F_INSTRUMENTATION_GENERAL_NMEA_0183_CONVERTER | 200 | Instrumentation/general systems | NMEA 0183 Converter |
| F_ENVIRONMENTAL_HVAC_NOT_AVAILABLE | 255 | Environmental (HVAC) systems | Not Available |
| F_DECK_CARGO_FISHING_NOT_AVAILABLE | 255 | Deck, cargo, and fishing equipment systems | Not Available |

### **Adapters**

None.

## Functionality

This global constant list provides a centralized set of function identifiers for Industry Group 4 (IG4) as defined in the ISO 11783 / ISOBUS standard. Each constant assigns a unique numeric value (BYTE) to a specific function within a system category. These constants are used in parameter group numbers (PGNs) to identify the source or purpose of a message, enabling correct routing and interpretation of data across the network.

## Technical Features

- All constants are of type `BYTE` and defined as global constants.
- Values range from 128 to 255, with `255` commonly used for "Not Available".
- The constants are grouped logically by system (e.g., Propulsion systems, Navigation systems, etc.).
- The list is intended to be used as a reference for mapping function codes in ISOBUS networks.

## State Overview

Not applicable. This is a constant definition, not a stateful block.

## Application Scenarios

- Referencing function codes when constructing or parsing ISOBUS messages.
- Assigning a function ID to an ECU in a network configuration.
- Ensuring consistent naming and values across multiple function blocks and applications.

## Comparison with Similar Blocks

This is not a typical function block but a global constant list. Similar lists exist for other industry groups (e.g., IG1, IG2, IG3, IG5) that define their own specific functions. The structure and usage are analogous, but the actual function codes and system categories differ per industry group.

## Conclusion

The IG4_NAME_Functions global constant list is an essential resource for developers working with ISOBUS networks in the marine and heavy-duty vehicle domain. It standardizes function identification, improving interoperability and maintainability of industrial control applications.
