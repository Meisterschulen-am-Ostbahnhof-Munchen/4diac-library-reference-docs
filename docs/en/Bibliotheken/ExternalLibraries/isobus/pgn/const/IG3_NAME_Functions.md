# IG3_NAME_Functions

![IG3_NAME_Functions](./IG3_NAME_Functions.svg)

* * * * * * * * * *

## Introduction

The **IG3_NAME_Functions** global constant block defines the standardized function codes (Function field of the ISO 11783 NAME) for machinery belonging to **Industry Group 3**. Industry Group 3 encompasses construction and agricultural equipment, including loaders, excavators, graders, pavers, and other specialized machinery. These constants provide symbolic names for numeric byte values (0x80 to 0xFF) that identify the specific function of an electronic control unit (ECU) or device on an ISOBUS (ISO 11783) network. They facilitate the development of interoperable applications by replacing hard-coded numeric values with human-readable, maintainable constant names.

## Interface Structure

This block is a **Global Constants** definition and does not contain any event inputs, event outputs, data inputs, data outputs, or adapter interfaces. It purely defines named constants that can be referenced across the 4diac development environment.

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

## Functionality

The block defines a set of byte constants grouped by **system categories** (Subsystem ID), following the ISOBUS NAME specification. Each constant has a value in the range **128 to 255** (0x80–0xFF) and corresponds to a specific function code for a given equipment type. The constants are organised as follows:

- **Non-specific system** – Functions applicable to any machine, such as:
  - `F_SUPPLEMENTAL_ENGINE_CONTROL_SENSING` = 128
  - `F_LASER_RECEIVER` = 129
  - `F_LOADER_CONTROL` = 135
  - `F_HYDRAULIC_VALVE_CONTROLLER` = 140
  - `F_JOYSTICK_CONTROL` = 141
  - `F_ALARM_DEVICE` = 146
  - and many others.
- **Skid Steer Loader** – Includes `F_SKID_STEER_LOADER_MAIN_CONTROLLER` = 128.
- **Crawler** – Includes `F_CRAWLER_BLADE_CONTROLLER` = 128.
- **Excavator** – Includes `F_EXCAVATOR_SLOPE_SENSOR` = 128.
- **Grader** – Includes `F_GRADER_HFWD_CONTROLLER` = 128.
- **Other equipment types** – For machines like articulated dump trucks, backhoes, forklifts, milling machines, pavers, etc., a generic `NOT_AVAILABLE` constant (value 255) is defined to be used until a specific function is assigned.

These constants allow developers to build ISOBUS applications that correctly identify the function of an ECU or device when constructing the NAME field, ensuring proper network communication and interpretation.

## Technical Features

- **Data Type**: All constants are of type `BYTE` (unsigned 8-bit integer).
- **Value Range**: Values are restricted between 128 (0x80) and 255 (0xFF), as per the ISOBUS standard for function codes.
- **Grouping**: Constants are logically grouped by machine type, using meaningful prefixes (e.g., `F_LASER_`, `F_SKID_STEER_LOADER_`).
- **Standard Compliance**: The values and naming follow the ISO 11783-3 specification for NAME function codes in Industry Group 3.
- **Reusability**: Because these are global constants, they can be used across multiple function blocks and systems within a project without duplication or risk of inconsistent values.

## State Overview

Not applicable – This block defines constants and does not have any state behavior.

## Application Scenarios

- **ISOBUS Electronic Control Unit (ECU) Development**: Use these constants to set the Function portion of the NAME message when configuring an ECU address assignment.
- **Diagnostic and Monitoring Applications**: Decode received NAME messages to identify the type of equipment connected.
- **Machine‑Specific Control Logic**: When implementing features for a specific machine (e.g., a skid steer loader), reference the corresponding constants to ensure the correct function identity.
- **Interoperability Testing**: Verify that generated NAME values comply with the industry standard by comparing against these predefined constants.

## Comparison with Similar Blocks

In the ISOBUS system, different industry groups define their own sets of function codes. This block is specifically for **Industry Group 3** (construction and agricultural machinery). For example:

- **IG1** (agricultural equipment) has its own function codes for tractors and implements.
- **IG2** is used for forestry machines.
- **IG3** (this block) covers construction equipment.

The main difference lies in the system categories and the specific functions defined for each machine type. This block provides a comprehensive list for the most common construction machines, while other groups may have fewer or different constants. Additionally, all groups include a `NOT_AVAILABLE` value (255) as a placeholder.

## Conclusion

The **IG3_NAME_Functions** global constant block is an essential resource for any ISOBUS‑based application targeting construction machinery. By providing clear, descriptive names for function codes, it improves code readability, reduces errors, and ensures compliance with the ISO 11783 standard. Its integration into 4diac‑IDE projects simplifies the development of robust, interoperable control systems for the construction industry.
