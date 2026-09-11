# Global_NAME_Functions

![Global_NAME_Functions](./Global_NAME_Functions.svg)

* * * * * * * * * *

## Introduction

`Global_NAME_Functions` is a global constant group (GlobalConstants) used within 4diac-IDE applications that interface with ISOBUS (ISO 11783) / SAE J1939 networks. It provides a set of named, industry non‑specific constants that represent the **Function** attribute of the **NAME** parameter group (J1939‑21 / ISO 11783‑7). Each byte value uniquely identifies the functional role of an electronic control unit (ECU) within a vehicle or machine network.

The constants act as a uniform vocabulary for classifying ECUs, allowing control logic to interpret NAME messages and to generate consistent, standards‑compliant identifiers. The group is compiled into the `isobus::pgn::const` package and can be referenced throughout 4diac‑IDE applications.

## Interface Structure

This element is a **GlobalConstants** block, not a function block, adapter, or subapplication. It exposes no runtime inputs, outputs, events, or adapters. Instead, it provides a collection of global constants that applications can read symbolically.

### **Event Inputs**

None – a global constant group has no event inputs.

### **Event Outputs**

None – a global constant group has no event outputs.

### **Data Inputs**

None – all entries are constants (`VAR_GLOBAL CONSTANT`) and are not modifiable at runtime.

### **Data Outputs**

The following BYTE constants are defined. Values follow the SAE J1939 / ISO 11783 NAME function code assignment and may be used wherever an ECU function identifier is required.

| Constant | Value | Description |
|----------|-------|-------------|
| `F_ENGINE` | 0 | Engine – typically the mechanical power source / engine management system. |
| `F_AUX_POWER_UNIT` | 1 | Auxiliary Power Unit (APU) – power source for operating systems without the prime drive engine. |
| `F_ELEC_PROPULSION_CONTROL` | 2 | Electric Propulsion Control – control system for electrically powered drive mechanisms. |
| `F_TRANSMISSION` | 3 | Transmission – alters speed vs. torque output of the engine. |
| `F_BATTERY_PACK_MONITOR` | 4 | Battery Pack Monitor – monitors charge, temperature, and power remaining. |
| `F_SHIFT_CONTROL_CONSOLE` | 5 | Shift Control/Console – determines and transmits desired gear, range, and operating mode. |
| `F_POWER_TAKEOFF_MAIN_OR_REAR` | 6 | Power Take‑Off (Main/Rear) – controls mechanical power for auxiliary items. |
| `F_AXLE_STEERING` | 7 | Axle Steering – adjusts attack angle as a function of steering. |
| `F_AXLE_DRIVE` | 8 | Axle Drive. |
| `F_BRAKES_SYSTEMCONTROL` | 9 | Brakes System Controller – electronically controls the service braking system. |
| `F_BRAKES_STEER_AXLE` | 10 | Brakes Steer Axle – actuates service brakes on a steered axle. |
| `F_BRAKES_DRIVE_AXLE` | 11 | Brakes Drive Axle – actuates service brakes on a drive axle. |
| `F_RETARDER_ENGINE` | 12 | Retarder Engine – controls engine retarder capabilities. |
| `F_RETARDER_DRIVELINE` | 13 | Retarder Driveline – controls driveline retarder capabilities. |
| `F_CRUISE_CONTROL` | 14 | Cruise Control – maintains vehicle speed at a fixed operator‑selectable value. |
| `F_FUEL_SYSTEM` | 15 | Fuel System – controls fuel flow from tank to engine and back. |
| `F_STEERING_CONTROL` | 16 | Steering Controller – controls steering in steer‑by‑wire systems. |
| `F_SUSPENSION_STEER_AXLE` | 17 | Suspension Steer Axle – control system for a steered axle suspension. |
| `F_SUSPENSION_DRIVE_AXLE` | 18 | Suspension Drive Axle – control system for a driven axle suspension. |
| `F_INSTRUMENT_CLUSTER` | 19 | Instrument Cluster – gauge display mounted in the cab. |
| `F_TRIP_RECORDER` | 20 | Trip Recorder – accumulates data versus travel. |
| `F_CAB_CLIMATE_CONTROL` | 21 | Cab Climate Control – controls climate within the cab. |
| `F_AERODYNAMIC_CONTROL` | 22 | Aerodynamic Control – modifies drag by altering body panels. |
| `F_VEHICLE_NAVIGATION` | 23 | Vehicle Navigation – physical location and driving instructions. |
| `F_VEHICLE_SECURITY` | 24 | Vehicle Security – compares operator‑provided data sequences against references. |
| `F_NETWORK_INTERCONNECT_UNIT` | 25 | Network Interconnect ECU – connects different network segments (bridge/gateway). |
| `F_BODY_CONTROLLER` | 26 | Body Controller – handles suspension control for body sections and body components. |
| `F_POWER_TAKEOFF_SECONDARY_OR_FRONT` | 27 | Power Take‑Off (Secondary/Front) – controls mechanical power for auxiliary items. |
| `F_OFF_VEHICLE_GATEWAY` | 28 | Off Vehicle Gateway – connects vehicle network(s) to off‑vehicle systems. |
| `F_VTERMINAL` | 29 | Virtual Terminal (in cab) – general purpose ISOBUS display. |
| `F_MANAGEMENT_COMPUTER` | 30 | Management Computer – manages vehicle systems such as the powertrain. |
| `F_PROPULSION_BATTERY_CHARGER` | 31 | Propulsion Battery Charger – charges propulsion batteries from an off‑board source. |
| `F_HEADWAY_CONTROLLER` | 32 | Headway Controller – forward‑looking collision avoidance/warning and speed matching. |
| `F_SYSTEM_MONITOR` | 33 | System Monitor. |
| `F_HYDRAULIC_PUMP_CONTROLLER` | 34 | Hydraulic Pump Controller – provides hydraulic power for installed equipment. |
| `F_SUSPENSION_SYSTEM_CONTROLLER` | 35 | Suspension System Controller – coordinates overall vehicle suspension. |
| `F_PNEUMATIC_SYSTEM_CONTROLLER` | 36 | Pneumatic System Controller. |
| `F_CAB_CONTROLLER` | 37 | Cab Controller – groups cab‑located vehicle functions. |
| `F_TIRE_PRESSURE_CONTROL` | 38 | Tire Pressure Control – centralised tire inflation. |
| `F_IGNITION_CONTROL_MODULE` | 39 | Ignition Control Module – alters engine ignition. |
| `F_SEAT_CONTROL` | 40 | Seat Control – controls operator/passenger seats. |
| `F_LIGHTING_OPERATOR_CONTROLS` | 41 | Lighting Operator Controls – transmits operator lighting control messages. |
| `F_WATER_PUMP_CONTROL` | 42 | Water Pump Control – controls a vehicle‑mounted water pump. |
| `F_TRANSMISSION_DISPLAY` | 43 | Transmission Display – displays transmission information (e.g., gear). |
| `F_EXHAUST_EMISSION_CONTROL` | 44 | Exhaust Emission Control. |
| `F_VEHICLE_DYNAMIC_STABILITY_CONTROL` | 45 | Vehicle Dynamic Stability Control. |
| `F_OIL_SENSOR_UNIT` | 46 | Oil Sensor Unit. |
| `F_INFORMATION_SYSTEM_CONTROLLER` | 47 | Information System Controller – manages vehicle application information. |
| `F_RAMP_CONTROL` | 48 | Ramp Control – loading/unloading (chairlift, ramps, lifts, tailgates). |
| `F_CLUTCH_CONVERTER_CONTROL` | 49 | Clutch/Converter Control – handles torque converter lock‑up. |
| `F_AUXILIARY_HEATER` | 50 | Auxiliary Heater – heating without the prime drive engine. |
| `F_FORWARD_LOOKING_COLLISION_WARNING_SYSTEM` | 51 | Forward‑Looking Collision Warning System – detects and warns of impending collisions. |
| `F_CHASSIS_CONTROLLER` | 52 | Chassis Controller – controls chassis components. |
| `F_ALTERNATOR_CHARGING_SYSTEM` | 53 | Alternator/Charging System – primary on‑board charging controller. |
| `F_COMMUNICATIONS_UNIT_CELLULAR` | 54 | Communications Unit, Cellular – communicates via the cellular telephone system. |
| `F_COMMUNICATIONS_UNIT_SATELLITE` | 55 | Communications Unit, Satellite – communicates via a satellite system. |
| `F_COMMUNICATIONS_UNIT_RADIO` | 56 | Communications Unit, Radio – communicates via terrestrial point‑to‑point. |
| `F_AUXILIARY_DEVICE` | 57 | Steering Column Unit – gathers operator inputs from steering column switches/levers. |
| `F_FAN_DRIVE_CONTROL` | 58 | Fan Drive Control – controls the main engine cooling fan. |
| `F_STARTER` | 59 | Starter – initiates rotation of a stopped engine. |
| `F_CAB_DISPLAY` | 60 | Cab Display – elaborate in‑cab display (greater than 30 ASCII characters). |
| `F_FILESERVER` | 61 | File Server / Printer – printing or file storage unit on the network. |
| `F_ON_BOARD_DIAGNOSTIC_UNIT` | 62 | On‑Board Diagnostic Unit – permanently mounted diagnostic tool. |
| `F_ENGINE_VALVE_CONTROLLER` | 63 | Engine Valve Controller – manipulates engine intake/exhaust valve actuation. |
| `F_ENDURANCE_BRAKING` | 64 | Endurance Braking – all devices enabling low‑wear speed reduction on long descents. |
| `F_GAS_FLOW_MEASUREMENT` | 65 | Gas Flow Measurement – provides gas flow rate and associated parameter measurements. |
| `F_I_O_CONTROLLER` | 66 | I/O Controller – reports and/or controls external input/output channels. |
| `F_ELECTRICAL_SYSTEM_CONTROLLER` | 67 | Electrical System Controller – includes load centers, fuseboxes, power distribution. |
| `F_AFTERTREATMENT_SYSTEM_GAS_MEASUREMENT` | 68 | Aftertreatment System Gas Measurement – measures gas properties (e.g., NOx, oxygen). |
| `F_ENGINE_EMISSION_AFTERTREATMENT_SYSTEM` | 69 | Engine Emission Aftertreatment System. |
| `F_AUXILIARY_REGENERATION_DEVICE` | 70 | Auxiliary Regeneration Device – used as part of an aftertreatment system. |
| `F_TRANSFER_CASE_CONTROL` | 71 | Transfer Case Control – selects number of drive wheels (e.g., 2WD/4WD). |
| `F_COOLANT_VALVE_CONTROLLER` | 72 | Coolant Valve Controller – controls coolant flow for thermal management. |
| `F_ROLLOVER_DETECTION_CONTROL` | 73 | Rollover Detection Control – detects vehicle rollover. |
| `F_LUBRICATION_SYSTEM` | 74 | Lubrication System – pumps lubricant to machine/vehicle joints. |
| `F_SUPPLEMENTAL_FAN` | 75 | Supplemental Fan – additional cooling fan beyond the primary fan. |
| `F_TEMPERATURE_SENSOR` | 76 | Temperature Sensor – devices which measure temperature. |
| `F_FUEL_PROPERTIES_SENSOR` | 77 | Fuel Properties Sensor – measures fuel properties. |
| `F_FIRE_SUPPRESSION_SYSTEM` | 78 | Fire Suppression System. |
| `F_POWER_SYSTEMS_MANAGER` | 79 | Power Systems Manager – manages power output of one or more power systems. |
| `F_ELECTRIC_POWERTRAIN` | 80 | Electric Powertrain – controls and coordinates an electric drive system. |
| `F_HYDRAULIC_POWERTRAIN` | 81 | Hydraulic Powertrain – controls and coordinates a hydraulic drive system. |
| `F_FILE_SERVER` | 82 | File Server – file storage unit on the network. |
| `F_PRINTER` | 83 | Printer – printing unit on the network. |
| `F_START_AID_DEVICE` | 84 | Start Aid Device – controls hardware (glow plug, grid heater) assisting engine start. |
| `F_ENGINE_INJECTION_CONTROL_MODULE` | 85 | Engine Injection Control Module – direct or port injection of fuel. |
| `F_EV_COMMUNICATION_CONTROLLER` | 86 | EV Communication Controller – manages connection to an external power source (EVSE). |
| `F_DRIVER_IMPAIRMENT_DEVICE` | 87 | Driver Impairment Device – prevents vehicle start due to driver impairment (e.g., alcohol interlock). |
| `F_ELECTRIC_POWER_CONVERTER` | 88 | Electric Power Converter – inverter/converter transforming AC/DC power. |
| `F_SUPPLY_EQUIPMENT_COMMUNICATION_CONTROLLER_SECC` | 89 | Supply Equipment Communication Controller (SECC) – part of an EV charging station. |
| `F_VEHICLE_ADAPTER_COMMUNICATION_CONTROLLER_VACC` | 90 | Vehicle Adapter Communication Controller (VACC) – controller inside the adapter between EVSE connector and vehicle inlet. |
| `F_ACCESSORY_ELECTRIC_MOTOR_CONTROLLER` | 91 | Accessory Electric Motor Controller – operates and monitors electrified accessories. |
| `F_CURRENT_SENSOR` | 92 | Current Sensor – measures electrical current. |
| `F_FUEL_CELL_SYSTEM` | 93 | Fuel Cell System – converts fuel (e.g., hydrogen) to electricity. |
| `F_AUXILIARY_DISPLAY` | 94 | Auxiliary Display – display inside/outside the cab, separate from Cab Display. |
| `F_NOT_AVAILABLE` | 255 | Not‑available function; placeholder for all undefined functions. |

### **Adapters**

None – a global constant group does not provide adapter interfaces.

## Functionality

The `Global_NAME_Functions` constants represent the **Function** field of the NAME parameter group used in J1939‑21 / ISO 11783‑7. Each ECU on an ISOBUS network announces its role through the NAME, and the Function code disambiguates which of the many possible vehicle/machine subsystems the node implements.

For 4diac‑IDE applications, these constants are used primarily in two ways:

- **Decoding** incoming NAME messages to identify the functional role of remote ECUs, enabling role‑based routing, diagnostics, or coordination logic.
- **Encoding** a local ECU's Function code when constructing a NAME message, ensuring the node appears with the correct classification on the network.

The values follow the industry‑standard, non‑specific function list and are kept as plain `BYTE` constants so they can be directly embedded in parameter groups (e.g., within the NAME 8‑byte data structure).

## Technical Features

- **Data Type:** All constants are of type `BYTE` (8‑bit unsigned integer).
- **Range:** Values 0 to 94 are assigned to specific functions; value 255 is used as the "not available" placeholder. Values 95 to 254 are not defined within this group.
- **Declaration:** Entries are declared as `VAR_GLOBAL CONSTANT`, making them read‑only and globally accessible within the project.
- **Packaging:** The constants are compiled into the `isobus::pgn::const` package, allowing include/reference from other 4diac‑IDE artifacts.
- **Compliance:** The mapping matches the industry non‑specific function definitions of ISOBUS/J1939, which is referenced as the "other" category in the ISO 11783 standard (distinct from industry‑specific groups, e.g., agricultural or construction).
- **Standard Reference:** The definitions align with the NAME Function assignment list published at isobus.net (function/name index).

## State Overview

Not applicable. `Global_NAME_Functions` is a pure constant group and does not contain state machines, internal variables, or runtime‑modifiable data. Its values are fixed at compile time and remain constant during application execution.

## Application Scenarios

- **ECU Role Identification:** In an ISOBUS master/slave controller, read an incoming NAME message and compare its Function byte against constants such as `F_ENGINE` or `F_CRUISE_CONTROL` to determine which type of device is present and how to interact with it.
- **NAME Construction:** When building a NAME parameter group for a local node (e.g., a general‑purpose display, a hydraulics controller, or an auxiliary power unit), assign the corresponding constant to the Function field for standards‑compliant network participation.
- **Diagnostic & Monitoring Tools:** Display meaningful textual descriptions of remote ECUs based on their function code, enabling service tools to show a human‑readable role for each address on the bus.
- **Function‑Specific Logic:** Implement distributed logic that adapts behavior based on the function of a counterpart, for example, coupling a headway controller with a brake system controller.
- **Placeholder & Unassigned Handling:** Use `F_NOT_AVAILABLE` as a safe default for nodes that do not (yet) claim a specific function, or for masking undefined codes in protocol stacks.

## Comparison with Similar Blocks

Within the ISOBUS/J1939 NAME specification, the Function field is only one of several attributes that together form the unique NAME of an ECU/component. Other global constant groups typically cover:

| Attribute | Purpose | Example Values |
|-----------|---------|----------------|
| **Function** (this group) | Identifies the subsystem role of the node | Engine, Transmission, Brakes, Display |
| **ECU Instance** | Differentiates multiple ECUs with the same function | 0, 1, 2… |
| **Industry Group** | Classifies the application domain (non‑specific, agriculture, construction, etc.) | Global, Agricultural, Construction/Mining |
| **Vehicle System** | Identifies the particular vehicle system instance | Powertrain, Chassis, Cab |
| **Arbitrary Address Capable** | Indicates if the unit supports arbitrary address claiming | yes/no |

While the Function group is "industry non‑specific", other constant groups assign values for specific industry‑related functions (e.g., agricultural implements). The distinction is important when building networks that must interoperate across standards: the non‑specific codes guarantee a common baseline, whereas industry‑specific codes extend the set for a vertical domain.

## Conclusion

`Global_NAME_Functions` provides a complete, standards‑aligned set of BYTE constants for the Function attribute of ISOBUS/J1939 NAME messages. By centralizing these codes in a global, read‑only constant group, 4diac‑IDE applications can reliably decode and encode the functional role of ECUs without scattering magic numbers through the control logic. Its coverage of 95 defined values plus the `F_NOT_AVAILABLE` placeholder makes it suitable for a wide range of vehicle and machinery automation tasks, from engine and transmission control to EV charging communication and auxiliary displays.