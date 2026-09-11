# IG5_Source_Addresses

![IG5_Source_Addresses](./IG5_Source_Addresses.svg)

* * * * * * * * * *
## Introduction
The **IG5_Source_Addresses** type is a global constants definition set used in the 4diac IDE environment. It provides a collection of predefined ISO 11783 (ISOBUS) Industry Group 5 source addresses (SA). These constants are employed to uniquely identify electronic control units (ECUs) on a CAN bus network in agricultural and forestry machinery applications, following the SAE J1939 / ISOBUS protocol.

The constants are declared as `BYTE` values and cover the source address range from 128 to 247, which is assigned to various engine, generator, and supplemental sensor processing units within Industry Group 5.

## Interface Structure
### **Event Inputs**
Not applicable – this is a global constants type and does not contain any event inputs.

### **Event Outputs**
Not applicable – this is a global constants type and does not contain any event outputs.

### **Data Inputs**
Not applicable – this is a global constants type and does not contain any data inputs.

### **Data Outputs**
Not applicable – the type exports constant values that are globally accessible, but they are not dynamic data outputs of a function block.

### **Global Constants**
The following table lists all constant declarations provided by this type:

| Constant Name | Type | Value (decimal) | Description |
|---------------|------|-----------------|-------------|
| `SA_RESERVED_128` | BYTE | 128 | Reserved range (128–207) for future SAE assignment – used for dynamic address assignment |
| `SA_RESERVED_208` | BYTE | 208 | Reserved range (208–229) for future assignment – used for individual preassigned addresses |
| `SA_GENERATOR_VOLTAGE_REGULATOR` | BYTE | 230 | Generator Voltage Regulator – controls generator output voltage |
| `SA_ENGINE_3` | BYTE | 231 | Engine #3 – ECU for the third engine within a system |
| `SA_ENGINE_4` | BYTE | 232 | Engine #4 – ECU for the fourth engine within a system |
| `SA_ENGINE_5` | BYTE | 233 | Engine #5 – ECU for the fifth engine within a system |
| `SA_GENERATOR_SET_CONTROLLER` | BYTE | 234 | Generator Set Controller – used for data collection and control of a generator system |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_1` | BYTE | 235 | Supplemental Sensor Processing Unit #1 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_2` | BYTE | 236 | Supplemental Sensor Processing Unit #2 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_3` | BYTE | 237 | Supplemental Sensor Processing Unit #3 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_4` | BYTE | 238 | Supplemental Sensor Processing Unit #4 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_5` | BYTE | 239 | Supplemental Sensor Processing Unit #5 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_6` | BYTE | 240 | Supplemental Sensor Processing Unit #6 |
| `SA_ENGINE_MONITOR_1` | BYTE | 241 | Engine Monitor #1 |
| `SA_ENGINE_MONITOR_2` | BYTE | 242 | Engine Monitor #2 |
| `SA_ENGINE_MONITOR_3` | BYTE | 243 | Engine Monitor #3 |
| `SA_ENGINE_MONITOR_4` | BYTE | 244 | Engine Monitor #4 |
| `SA_ENGINE_MONITOR_5` | BYTE | 245 | Engine Monitor #5 |
| `SA_ENGINE_MONITOR_6` | BYTE | 246 | Engine Monitor #6 |
| `SA_ENGINE_MONITOR_7` | BYTE | 247 | Engine Monitor #7 |

## Functionality
This global constants type centralizes all Industry Group 5 source addresses in one location. By referencing these named constants, applications and function blocks can avoid hard-coded numeric CAN identifiers, making the system configuration more readable, maintainable, and less error-prone. Each constant corresponds to a specific SA value defined by the ISOBUS/J1939 standard for a particular ECU function. The constants are designed to be used in network layer logic, address claiming procedures, and PGN (Parameter Group Number) filtering within ISOBUS applications.

## Technical Features
- **Data Type:** All constants are of type `BYTE` (8-bit unsigned integer), fitting the SA range of 0–255 as defined in the J1939 standard.
- **Address Range:** The defined values cover the Industry Group 5 range (128–247), including reserved addresses, generator-related units, and engine monitoring ECUs.
- **Global Accessibility:** As a `GLOBALCONSTANTS` type, the constants are globally visible throughout the 4diac project once instantiated, enabling consistent use across multiple function blocks and applications.
- **Read-Only:** The constants are immutable; their values cannot be changed at runtime.
- **Standard Compliance:** Values follow the SAE J1939 / ISO 11783 source address assignments for Industry Group 5, ensuring protocol interoperability.

## State Overview
Not applicable – this is a global constants type without any internal state machine or runtime behavior. It provides static values only.

## Application Scenarios
- **ISOBUS Network Configuration:** Use these constants when configuring the source address (SA) for an ECU in a tractor or implement network.
- **Address Claiming Logic:** Reference the constants in function blocks that perform dynamic address claiming to ensure the correct SA is used for a specific device type.
- **CAN Message Filtering:** Use the constants to filter incoming messages based on the source address of the transmitting ECU.
- **Multi-Engine Systems:** Assign the correct engine SA (e.g., `SA_ENGINE_3`, `SA_ENGINE_4`) when managing multiple engine ECUs on the same bus.
- **Generator Monitoring:** Refer to `SA_GENERATOR_VOLTAGE_REGULATOR` and `SA_GENERATOR_SET_CONTROLLER` to identify generator-related messages.
- **Engine Diagnostics:** Use `SA_ENGINE_MONITOR_1` through `SA_ENGINE_MONITOR_7` to distinguish between up to seven engine monitoring units.

## Comparison with Similar Blocks
Unlike function blocks (FBs) or adapters, `IG5_Source_Addresses` is a global constants type, meaning it has no event processing, no execution states, and no data flow. Its purpose is purely declarative. Compared to locally declared variables inside a function block, this global constants type offers:

- **Reusability:** Defined once and used across many FBs and applications.
- **Maintainability:** Updating a constant value in one location propagates to all references.
- **Safety:** Prevents accidental modification during runtime.
- **Standardization:** Enforces consistent naming and values across the entire project.

Other global constants types may exist for other industry groups (e.g., IG1–IG4), but this one is specifically scoped to Industry Group 5, avoiding naming clashes and keeping the address space organized.

## Conclusion
The `IG5_Source_Addresses` global constants type provides a clean, standardized way to manage ISO 11783 Industry Group 5 source addresses in a 4diac project. Its fixed, well-documented constant set simplifies development of ISOBUS-compliant applications, improves code readability, and ensures that source addresses are always used consistently across distributed function blocks. It is an essential building block for any agricultural or forestry machinery control system operating on an ISOBUS/CAN network.