# IG1_Source_Addresses

![IG1_Source_Addresses](./IG1_Source_Addresses.svg)

* * * * * * * * * *

## Introduction

The **IG1_Source_Addresses** GlobalConstants package defines a set of named constant values representing the ISO 11783 (ISOBUS) **Industry Group 1 specific source addresses** for the J1939-style network. These constants map human‑readable device identifiers (e.g., `SA_AUTOMATED_DRIVING_CONTROLLER_1`) to their assigned byte values in the range 128–247, a range reserved for Industry Group 1 (non‑standard, agriculture/forestry oriented) ECU source addresses.

The constant definitions are provided as global read‑only variables of type `BYTE`, enabling embedded logic blocks and application components to reference specific source addresses by name rather than by numeric literals. This improves code readability, reduces the risk of typing errors, and centralizes the address mapping for maintenance.

The package is organized as a `GLOBALCONSTANTS` block in compliance with the IEC 61499‑1 standard and is intended to be included in projects that communicate over ISOBUS networks.

## Interface Structure

Since this element is a **GlobalConstants** definition and not a function block, it does not expose event inputs, event outputs, data inputs, data outputs, or adapters. Instead, it provides a set of **global constant data elements** of type `BYTE` that can be used throughout an application. The following table lists all defined constants with their assigned values and comments.

### Data Constants

| Constant Name | Value (BYTE) | Description |
|---|---|---|
| `SA_RESERVED_128` | 128 | Reserved for future assignment by SAE; available for self‑configurable ECUs |
| `SA_AUTOMATED_DRIVING_CONTROLLER_2` | 156 | Automated Driving Controller 2 (second instance) |
| `SA_ELECTRIC_PROPULSION_CONTROL_UNIT_3` | 157 | Electric Propulsion Control Unit #3 |
| `SA_AUTOMATED_DRIVING_CONTROLLER_1` | 158 | Automated Driving Controller 1 |
| `SA_ROADWAY_INFORMATION_SYSTEM` | 159 | Roadway Information System |
| `SA_ADVANCED_EMERGENCY_BRAKING_SYSTEM` | 160 | Advanced Emergency Braking System |
| `SA_FIFTH_WHEEL_SMART_SYSTEMS` | 161 | Fifth Wheel Smart Systems |
| `SA_SLOPE_SENSOR` | 162 | Slope Sensor |
| `SA_CATALYST_FLUID_SENSOR` | 163 | Catalyst Fluid Sensor |
| `SA_ON_BOARD_DIAGNOSTIC_UNIT_2` | 164 | On Board Diagnostic Unit #2 |
| `SA_REAR_STEERING_AXLE_CONTROLLER_2` | 165 | Rear Steering Axle Controller #2 |
| `SA_REAR_STEERING_AXLE_CONTROLLER_3` | 166 | Rear Steering Axle Controller #3 |
| `SA_INSTRUMENT_CLUSTER_2` | 167 | Instrument Cluster #2 (auxiliary display) |
| `SA_TRAILER_5_BRIDGE` | 168 | Trailer #5 Bridge |
| `SA_TRAILER_5_LIGHTING_ELECTRICAL` | 169 | Trailer #5 Lighting‑Electrical |
| `SA_TRAILER_5_BRAKES_ABS_EBS` | 170 | Trailer #5 Brakes (ABS‑EBS) |
| `SA_TRAILER_5_REEFER` | 171 | Trailer #5 Reefer |
| `SA_TRAILER_5_CARGO` | 172 | Trailer #5 Cargo |
| `SA_TRAILER_5_CHASSIS_SUSPENSION` | 173 | Trailer #5 Chassis‑Suspension |
| `SA_OTHER_TRAILER_5_DEVICES` | 174 | Other Trailer #5 Devices |
| `SA_OTHER_TRAILER_5_DEVICES_175` | 175 | Other Trailer #5 Devices (subnetwork) |
| `SA_TRAILER_4_BRIDGE` | 176 | Trailer #4 Bridge |
| `SA_TRAILER_4_LIGHTING_ELECTRICAL` | 177 | Trailer #4 Lighting‑Electrical |
| `SA_TRAILER_4_BRAKES_ABS_EBS` | 178 | Trailer #4 Brakes (ABS‑EBS) |
| `SA_TRAILER_4_REEFER` | 179 | Trailer #4 Reefer |
| `SA_TRAILER_4_CARGO` | 180 | Trailer #4 Cargo |
| `SA_TRAILER_4_CHASSIS_SUSPENSION` | 181 | Trailer #4 Chassis‑Suspension |
| `SA_OTHER_TRAILER_4_DEVICES` | 182 | Other Trailer #4 Devices |
| `SA_OTHER_TRAILER_4_DEVICES_183` | 183 | Other Trailer #4 Devices (subnetwork) |
| `SA_TRAILER_3_BRIDGE` | 184 | Trailer #3 Bridge |
| `SA_TRAILER_3_LIGHTING_ELECTRICAL` | 185 | Trailer #3 Lighting‑Electrical |
| `SA_TRAILER_3_BRAKES_ABS_EBS` | 186 | Trailer #3 Brakes (ABS‑EBS) |
| `SA_TRAILER_3_REEFER` | 187 | Trailer #3 Reefer |
| `SA_TRAILER_3_CARGO` | 188 | Trailer #3 Cargo |
| `SA_TRAILER_3_CHASSIS_SUSPENSION` | 189 | Trailer #3 Chassis‑Suspension |
| `SA_OTHER_TRAILER_3_DEVICES` | 190 | Other Trailer #3 Devices |
| `SA_OTHER_TRAILER_3_DEVICES_191` | 191 | Other Trailer #3 Devices (subnetwork) |
| `SA_TRAILER_2_BRIDGE` | 192 | Trailer #2 Bridge |
| `SA_TRAILER_2_LIGHTING_ELECTRICAL` | 193 | Trailer #2 Lighting‑Electrical |
| `SA_TRAILER_2_BRAKES_ABS_EBS` | 194 | Trailer #2 Brakes (ABS‑EBS) |
| `SA_TRAILER_2_REEFER` | 195 | Trailer #2 Reefer |
| `SA_TRAILER_2_CARGO` | 196 | Trailer #2 Cargo |
| `SA_TRAILER_2_CHASSIS_SUSPENSION` | 197 | Trailer #2 Chassis‑Suspension |
| `SA_OTHER_TRAILER_2_DEVICES` | 198 | Other Trailer #2 Devices |
| `SA_OTHER_TRAILER_2_DEVICES_199` | 199 | Other Trailer #2 Devices (subnetwork) |
| `SA_TRAILER_1_BRIDGE` | 200 | Trailer #1 Bridge |
| `SA_TRAILER_1_LIGHTING_ELECTRICAL` | 201 | Trailer #1 Lighting‑Electrical |
| `SA_TRAILER_1_BRAKES_ABS_EBS` | 202 | Trailer #1 Brakes (ABS‑EBS) |
| `SA_TRAILER_1_REEFER` | 203 | Trailer #1 Reefer |
| `SA_TRAILER_1_CARGO` | 204 | Trailer #1 Cargo |
| `SA_TRAILER_1_CHASSIS_SUSPENSION` | 205 | Trailer #1 Chassis‑Suspension |
| `SA_OTHER_TRAILER_1_DEVICES` | 206 | Other Trailer #1 Devices |
| `SA_OTHER_TRAILER_1_DEVICES_207` | 207 | Other Trailer #1 Devices (subnetwork) |
| `SA_RESERVED_208` | 208 | Reserved for future assignment by SAE (208–227) |
| `SA_STEERING_INPUT_UNIT` | 228 | Steering Input Unit |
| `SA_BODY_CONTROLLER_2` | 229 | Body Controller #2 |
| `SA_BODY_TO_VEHICLE_INTERFACE_CONTROL` | 230 | Body‑to‑Vehicle Interface Control |
| `SA_ARTICULATION_TURNTABLE_CONTROL` | 231 | Articulation Turntable Control |
| `SA_FORWARD_ROAD_IMAGE_PROCESSOR` | 232 | Forward Road Image Processor |
| `SA_DOOR_CONTROLLER_3` | 233 | Door Controller #3 |
| `SA_DOOR_CONTROLLER_4` | 234 | Door Controller #4 |
| `SA_TRACTOR_TRAILER_BRIDGE_2` | 235 | Tractor/Trailer Bridge #2 |
| `SA_DOOR_CONTROLLER_1` | 236 | Door Controller #1 (driver side) |
| `SA_DOOR_CONTROLLER_2` | 237 | Door Controller #2 (co‑driver side) |
| `SA_TACHOGRAPH` | 238 | Tachograph |
| `SA_ELECTRIC_PROPULSION_CONTROL_UNIT_1` | 239 | Electric Propulsion Control Unit #1 |
| `SA_ELECTRIC_PROPULSION_CONTROL_UNIT_2` | 240 | Electric Propulsion Control Unit #2 |
| `SA_WWH_OBD_TESTER` | 241 | WWH‑OBD Tester |
| `SA_ELECTRIC_PROPULSION_CONTROL_UNIT_4` | 242 | Electric Propulsion Control Unit #4 |
| `SA_BATTERY_PACK_MONITOR_1` | 243 | Battery Pack Monitor #1 |
| `SA_BATTERY_PACK_MONITOR_2_APU_4` | 244 | Battery Pack Monitor #2 / APU #4 |
| `SA_BATTERY_PACK_MONITOR_3_APU_3` | 245 | Battery Pack Monitor #3 / APU #3 |
| `SA_BATTERY_PACK_MONITOR_4_APU_2` | 246 | Battery Pack Monitor #4 / APU #2 |
| `SA_AUXILIARY_POWER_UNIT_APU_1` | 247 | Auxiliary Power Unit (APU) #1 |

### Event Inputs

Not applicable – this is a global constant definition, not a function block.

### Event Outputs

Not applicable.

### Data Inputs

Not applicable.

### Data Outputs

Not applicable – the constants are global read‑only values that may be referenced by any application component.

### Adapters

Not applicable.

## Functionality

The primary functionality of `IG1_Source_Addresses` is to provide a **centralized, named mapping** of ISOBUS source addresses for devices belonging to Industry Group 1. In an ISOBUS network, every electronic control unit must use a unique source address (0–253) to identify itself in transmitted messages. Industry Group 1 covers agricultural and forestry equipment, and the SAE J1939‑based address allocation assigns specific addresses to device classes within this group.

By referencing the constants defined here, an application can:

- Assign a source address to a local ECU during network initialization.
- Filter incoming messages based on the source address of the transmitting device.
- Configure diagnostic or monitoring functions that need to distinguish between different controllers.
- Ensure that address values are consistent across the entire project without hard‑coding numeric values.

The constant names follow a descriptive convention (prefix `SA_` followed by the device designation), which makes the purpose of each address immediately understandable in code.

## Technical Features

- **Data type:** All constants are of type `BYTE` (unsigned 8‑bit integer), which matches the J1939/ISOBUS source address width.
- **Address range:** Covers the Industry Group 1 range from 128 to 247, including reserved gaps (128–155, 208–227) and the explicitly defined addresses 156–247.
- **Reserved entries:** Two constants (`SA_RESERVED_128` and `SA_RESERVED_208`) explicitly declare the start of reserved ranges, allowing applications to check for reserved addresses.
- **Self‑configuration support:** The comment for `SA_RESERVED_128` notes that the range 128–155 is "available for use by self‑configurable ECUs," indicating support for dynamic address assignment.
- **Trailer subsystem addressing:** For each of the five possible trailers, seven addresses are defined (bridge, lighting/electrical, brakes/ABS‑EBS, reefer, cargo, chassis/suspension, and other devices), allowing a full topology of towed equipment to be addressed.
- **Electric vehicle integration:** Multiple addresses for electric propulsion control units (ECUs #1–#4) and battery pack monitors are included, reflecting modern electric‑drivetrain requirements.
- **IEC 61499 compliance:** The constants are declared within a `GlobalConstants` block, conforming to the IEC 61499‑1 standard for global data in distributed industrial‑process measurement and control systems.

## State Overview

This element is **stateless**. It does not implement a state machine or exhibit runtime behavior – it simply provides compile‑time constant values. Once defined, the values are immutable and available throughout the application lifecycle. There are no states to transition between, and no events trigger changes to the constants.

## Application Scenarios

- **ISOBUS controller development:** A tractor or implement ECU that needs to claim a source address can assign the address from this constant set during startup, e.g., `SA_AUTOMATED_DRIVING_CONTROLLER_1` (158) for an autonomous driving controller.
- **Message filtering and routing:** A gateway or bridge (e.g., `SA_TRACTOR_TRAILER_BRIDGE_2`) can use these constants to discriminate messages from different trailer subsystems and forward them to appropriate network segments.
- **Diagnostics:** An on‑board diagnostic unit (`SA_ON_BOARD_DIAGNOSTIC_UNIT_2`, `SA_WWH_OBD_TESTER`) can reference these constants to implement diagnostic services that query specific ECUs.
- **Electric vehicle fleets:** With addresses for propulsion control units and battery monitors, an electric tractor or implement can use these constants to manage the communication with multiple high‑voltage components.
- **Multi‑trailer configurations:** Agricultural operations that pull one or more trailers (e.g., grain carts, sprayers) can use the trailer‑specific addresses to monitor and control lighting, brakes, reefers, and cargo conditions for each trailer individually.

## Comparison with Similar Blocks

- **vs. generic constant definitions:** Most IEC 61499 projects define constants individually within function blocks or applications. `IG1_Source_Addresses` centralizes a large set of related constants in one global namespace, improving consistency and reducing duplication.
- **vs. hard‑coded numeric values:** Using named constants eliminates the risk of typos and makes code self‑documenting. Numeric address assignments are often scattered and error‑prone when repeated.
- **vs. a function block with output pins:** A function block could expose source address values via output data ports, but it would need to be instantiated and would introduce runtime dependency. Global constants are available immediately without instantiation, making them more efficient for constant data.
- **vs. other ISOBUS address constant sets:** A similar GlobalConstants block for other industry groups (e.g., Group 2 addresses 0–127 or Group 3 addresses 248–253) would complement this file. This block specifically covers Group 1, while other groups would contain their own dedicated constants.

## Conclusion

The **IG1_Source_Addresses** GlobalConstants block is an essential building block for any ISOBUS‑based application developed in 4diac‑ide. It provides a comprehensive, well‑documented set of byte constants representing Industry Group 1 source addresses, covering propulsion, braking, towing, trailer, and body control systems. By using these named constants, developers ensure that their applications are readable, maintainable, and consistent with the SAE J1939 address allocation scheme. The stateless nature and global availability of the constants make them easy to integrate into any IEC 61499 application, improving both development efficiency and network interoperability.
