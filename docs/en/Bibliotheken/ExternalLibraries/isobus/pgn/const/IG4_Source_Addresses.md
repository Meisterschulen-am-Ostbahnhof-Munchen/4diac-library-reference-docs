# IG4_Source_Addresses

![IG4_Source_Addresses](./IG4_Source_Addresses.svg)

* * * * * * * * * *
## Introduction

**IG4_Source_Addresses** is a global constant definition block within the **isobus::pgn::const** package, providing standardized ISOBUS (ISO 11783) source address constants for **Industry Group 4** (Marine applications). It defines 22 named constants covering reserved address ranges and assigned device addresses for marine engine systems, displays, transmissions, and propulsion sensor gateways. These constants are used as symbolic references instead of raw numeric values, improving code readability and maintainability in ISOBUS-based applications targeting marine powertrain and monitoring equipment.

## Interface Structure

This element is a **GlobalConstants** container, not a standard function block. It does not expose any event, data, or adapter ports. Instead, it publishes named constants that can be referenced globally within the application project.

### **Event Inputs**

None. Constants blocks do not have event inputs.

### **Event Outputs**

None. Constants blocks do not have event outputs.

### **Data Inputs**

None. The values are statically defined at compile time.

### **Data Outputs**

None. The values are statically defined at compile time.

### **Adapters**

None. This block does not contain adapters.

The following table lists all defined constants (all of type `BYTE`):

| Constant Name | Value | Description |
|---|---|---|
| `SA_RESERVED_128` | 128 | Reserved (128–207) for dynamic address assignment |
| `SA_RESERVED_208` | 208 | Reserved (208–227) for individual preassigned addresses |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_1` | 228 | Propulsion Sensor Hub &amp; Gateway #1 |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_2` | 229 | Propulsion Sensor Hub &amp; Gateway #2 |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_3` | 230 | Propulsion Sensor Hub &amp; Gateway #3 |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_4` | 231 | Propulsion Sensor Hub &amp; Gateway #4 |
| `SA_TRANSMISSION_3` | 232 | Transmission ECU #3 |
| `SA_TRANSMISSION_4` | 233 | Transmission ECU #4 |
| `SA_TRANSMISSION_5` | 234 | Transmission ECU #5 |
| `SA_TRANSMISSION_6` | 235 | Transmission ECU #6 |
| `SA_DISPLAY_1_FOR_PROTECTION_SYSTEM_FOR_MARINE_ENGINES` | 236 | Display #1 for Marine Engine Protection System |
| `SA_PROTECTION_SYSTEM_FOR_MARINE_ENGINES` | 237 | Protection System ECU for Marine Engines |
| `SA_ALARM_SYSTEM_CONTROL_1_FOR_MARINE_ENGINES` | 238 | Alarm System Control #1 for Marine Engines |
| `SA_ENGINE_3` | 239 | Engine ECU #3 |
| `SA_ENGINE_4` | 240 | Engine ECU #4 |
| `SA_ENGINE_5` | 241 | Engine ECU #5 |
| `SA_MARINE_DISPLAY_1` | 242 | Marine Display #1 |
| `SA_MARINE_DISPLAY_2` | 243 | Marine Display #2 |
| `SA_MARINE_DISPLAY_3` | 244 | Marine Display #3 |
| `SA_MARINE_DISPLAY_4` | 245 | Marine Display #4 |
| `SA_MARINE_DISPLAY_5` | 246 | Marine Display #5 |
| `SA_MARINE_DISPLAY_6` | 247 | Marine Display #6 |

## Functionality

The constant set implements the ISOBUS source address scheme for Industry Group 4. It covers:

- **Reserved ranges** (128–207 and 208–227) that are kept available for SAE future assignments, distinguishing between dynamically self-configurable addresses and individually preassigned addresses.
- **Propulsion sensor hub gateways** (228–231) for up to four sensor hub/gateway devices.
- **Transmission ECUs** (232–235) for the third through sixth transmission in a multi-transmission marine propulsion system. The first two transmissions are typically defined in other Industry Group constant sets.
- **Marine engine protection and alarm systems** (236–238), including a dedicated display for the protection system.
- **Engine ECUs** (239–241) for engines #3 through #5. Engines #1 and #2 use addresses from lower Industry Group ranges (as defined by the ISOBUS standard).
- **Marine displays** (242–247) for up to six display devices.

## Technical Features

- All constants are of type `BYTE` with initial values within the 0–255 range, matching the single-byte address field of ISOBUS (ISO 11783) and SAE J1939.
- The constants are declared as `VAR_GLOBAL CONSTANT`, making them immutable and globally accessible across all function blocks in the project.
- Naming follows a descriptive `SA_` prefix convention, directly mapping the symbolic name to the numeric source address, eliminating the need to memorize raw address values.
- The block is part of the `isobus::pgn::const` package, providing a modular namespace for ISOBUS-related constants.

## State Overview

This is a passive data container. It has no runtime state, no internal algorithms, and no transition logic. The values are fixed at compile time and remain constant during execution. Therefore, no state machine or operational states apply.

## Application Scenarios

- **Marine engine monitoring systems** – determining the correct source address for engine ECUs when configuring a J1939/ISOBUS network with multiple engines.
- **Multi-transmission marine propulsion control** – assigning addresses for the third through sixth transmission ECUs.
- **Alarm and protection systems** – configuring alarm system control units, protection system displays, and the protection system ECU itself.
- **Display integration** – addressing up to six marine displays on the same bus.
- **Development and test setups** – using the reserved constants to validate dynamic address assignment logic or preconfigured address allocation.

## Comparison with Similar Blocks

| Aspect | IG4_Source_Addresses | IG0/IG1/IG2/IG3 Source Address Sets |
|---|---|---|
| Industry Group | 4 (Marine) | 0–3 (Other equipment classes) |
| Address range | 128–247 (partially, with gaps) | Varies per group |
| Content focus | Marine engines, transmissions, displays, protection systems | Agriculture, construction, forestry, etc. |
| Reserved addresses | 128–227 (two separate ranges) | Group-specific |

The structure of this constant set follows the same pattern as other Industry Group source address blocks in the `isobus::pgn::const` package. The main difference is the specific device types and the address ranges assigned by the ISOBUS standard for marine applications.

## Conclusion

**IG4_Source_Addresses** provides a complete, standardized set of source address constants for ISOBUS marine applications. It eliminates hard-coded numeric addresses, reduces configuration errors, and ensures compliance with the ISOBUS address allocation scheme. By using these constants in PGN construction and network filtering logic, developers can reliably build interoperable marine powertrain and monitoring systems. The block is particularly useful when a marine bus contains multiple engines, transmissions, sensor hubs, or displays that must be uniquely addressed without conflicts.