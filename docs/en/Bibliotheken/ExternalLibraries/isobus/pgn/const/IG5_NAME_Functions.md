# IG5_NAME_Functions

![IG5_NAME_Functions](./IG5_NAME_Functions.svg)

* * * * * * * * * *
## Introduction
This document describes the **IG5_NAME_Functions** global constant set, which defines Industry Group 5 (IG5) specific function codes for ISOBUS (ISO 11783) implementation. These constants are used to identify the function of devices and components within the "Industrial-Process Control - Stationary (Gen-Sets)" system category, as well as a general "Not Available" placeholder. The constant set provides a standardized naming convention for referencing these function codes in control logic and data mapping.

## Interface Structure
Since **IG5_NAME_Functions** is a global constant definition, it does not contain event inputs, event outputs, data inputs, data outputs, or adapters. It exposes a set of named `BYTE` constants that can be referenced in IEC 61499 applications or 4diac-IDE projects.

### **Event Inputs**
Not applicable – this global constant set does not process events.

### **Event Outputs**
Not applicable – this global constant set does not generate events.

### **Data Inputs**
Not applicable – this global constant set does not have data inputs.

### **Data Outputs**
Not applicable – this global constant set does not have data outputs.

### **Adapters**
Not applicable – this global constant set does not include adapters.

## Functionality
The purpose of this global constant set is to provide symbolic names for function codes defined by the ISOBUS Industry Group 5 specification. These codes are used to describe the role or operation of a device connected to an ISOBUS network. By using the constants, applications can avoid hard-coded numeric values and improve readability and maintainability.

The constants defined in this set are:

| Constant Name | Value | Description |
|---------------|-------|-------------|
| `F_INDUSTRIAL_GEN_SET_SUPPLEMENTAL_ENGINE_CONTROL_SENSING` | 128 | Reserved for supplemental engine control sensing in gen-sets. |
| `F_INDUSTRIAL_GEN_SET_GENERATOR_SET_CONTROLLER` | 129 | Generator set controller for collecting data and controlling the generator. |
| `F_INDUSTRIAL_GEN_SET_GENERATOR_VOLTAGE_REGULATOR` | 130 | Generator voltage regulator for maintaining output voltage. |
| `F_INDUSTRIAL_GEN_SET_CHOKE_ACTUATOR` | 131 | Choke actuator for controlling air flow on a gas engine. |
| `F_WELL_STIMULATION_PUMP` | 132 | Well stimulation pump used in oil and gas drilling applications. |
| `F_INDUSTRIAL_GEN_SET_NOT_AVAILABLE` | 255 | Placeholder for gen-set function when no explicit function is assigned. |
| `F_NOT_AVAILABLE` | 255 | General placeholder for any device whose function is not yet defined. |

These constants are used to populate the `Function` field in ISOBUS parameter groups (PGNs) that transmit device information or operational status.

## Technical Features
- **Data Type**: All constants are of type `BYTE` (8-bit unsigned integer).
- **Compatibility**: The values correspond to the ISO 11783-6 standard for function codes in the industrial/process control domain.
- **Naming Convention**: Each constant follows the pattern `F_<SYSTEM>_<FUNCTION>` to clearly indicate its intended application area.
- **Reusability**: The constants can be directly referenced in IEC 61499 function block implementations, ST (Structured Text) algorithms, or any other 4diac-IDE programming interface.
- **Documentation**: Each constant carries an inline comment that describes its system and function context.

## State Overview
This global constant set does not have a state machine; it defines static values that remain unchanged during runtime. No state transitions, initialization sequences, or internal states are applicable.

## Application Scenarios
The `IG5_NAME_Functions` global constants are typically used in automation projects that involve:
- **Generator Set Control**: Applications monitoring and controlling stationary diesel/gas generator sets used for emergency power or continuous operation.
- **ISOBUS Communication**: Systems that implement ISOBUS PGNs to exchange device function information, such as the "Name" object or function-specific parameters.
- **Oil & Gas Drilling**: Integration of well stimulation pumps that communicate their operational parameters over an ISOBUS network.
- **Device Identification**: Assigning function codes to allow a central controller (e.g., a generator set controller) to identify the type and role of connected peripherals.

In 4diac-IDE, these constants can be imported into a project and used alongside function blocks that require a `BYTE` input for function selection or reporting.

## Comparison with Similar Blocks
Since this is a global constant definition rather than a function block, there is no direct comparison to FB types. However, similar constant sets exist for other Industry Groups (e.g., IG1–IG4, IG6) in the ISOBUS standard. The key difference is the set of specific functions defined for gen-set and oil/gas applications in IG5. If an application requires a different industry group, a corresponding global constant set must be used. This set provides the necessary values for typical IG5 equipment but does not include function codes for other domains.

## Conclusion
The `IG5_NAME_Functions` global constants offer a clean and standardized way to reference ISOBUS function codes for industrial gen-sets and related equipment. By using these constants, developers can ensure compatibility with the ISO 11783 standard, reduce the risk of data misinterpretation, and make their control logic more readable and maintainable. The set covers the most common functions in this domain and provides a universal "Not Available" fallback, making it a practical resource for ISOBUS-based automation projects.