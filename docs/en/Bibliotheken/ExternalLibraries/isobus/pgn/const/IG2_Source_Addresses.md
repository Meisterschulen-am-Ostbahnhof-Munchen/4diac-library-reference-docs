# IG2_Source_Addresses

![IG2_Source_Addresses](./IG2_Source_Addresses.svg)

* * * * * * * * * *
## Introduction

The `IG2_Source_Addresses` global constant definition provides a standardized set of source addresses for devices operating in Industry Group 2 (IG2) on an ISO 11783 (ISOBUS) network. These addresses are reserved for specific control functions and are used to uniquely identify devices or services within the network. The constants are defined as `BYTE` values and are intended to be used as preferred or fixed source addresses according to the ISO 11783 standard.

## Interface Structure

This global constant type does not contain any event or data inputs. It exposes a collection of constant values as read‑only data outputs, which are globally accessible to function blocks and applications within the 4diac‑IDE environment.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following table lists the constant data outputs provided by this global constant type. Each entry represents a fixed source address value.

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `SA_RESERVED_128` | `BYTE` | 128 | Reserved source addresses 128–235 for self‑configurable devices. |
| `SA_DATA_LOGGER` | `BYTE` | 236 | Data Logger – a server providing LOG functionality per ISO 11783‑10. |
| `SA_TIM_SERVER` | `BYTE` | 237 | TIM Server – represents a Tractor Implement Management (TIM) server. |
| `SA_SEQUENCE_CONTROLLER` | `BYTE` | 238 | Sequence Controller – master in the Sequence Control System (ISO 11783‑14). |
| `SA_POSITION_CONTROL` | `BYTE` | 239 | Position Control – multiple axial position control of movable device elements. |
| `SA_TRACTOR_ECU` | `BYTE` | 240 | Tractor ECU – gateway between tractor and implement bus, representing the tractor. |
| `SA_TAILINGS_MONITORING` | `BYTE` | 241 | Tailings Monitoring – monitors quantity of unthreshed material returned to threshing machine. |
| `SA_HEADER_CONTROL` | `BYTE` | 242 | Header Control – controls header reel rate and material delivery rate. |
| `SA_PRODUCT_LOSS_MONITORING` | `BYTE` | 243 | Product Loss Monitoring – monitors produce loss in harvest processes. |
| `SA_PRODUCT_MOISTURE_SENSING` | `BYTE` | 244 | Product Moisture Sensing – monitors produce moisture content. |
| `SA_NON_VIRTUAL_TERMINAL_DISPLAY_IMPLEMENT_BUS` | `BYTE` | 245 | Non‑Virtual Terminal Display (Implement Bus) – cab display connected to implement bus. |
| `SA_OPERATOR_CONTROLS_MACHINE_SPECIFIC` | `BYTE` | 246 | Operator Controls – machine‑specific non‑auxiliary operator control inputs. |
| `SA_TASK_CONTROL_MAPPING_COMPUTER` | `BYTE` | 247 | Task Control (Mapping Computer) – responsible for sending, receiving and logging process data. |

### **Adapters**

None.

## Functionality

The `IG2_Source_Addresses` global constants are used to assign fixed, well‑known source addresses to ISOBUS‑compliant devices. By referencing these constants, developers ensure that their function blocks or applications use the correct addresses as defined by the ISO 11783 standard. The constants are grouped together to provide a single, maintainable definition for all IG2‑specific source addresses, avoiding hard‑coded magic numbers in the application logic.

## Technical Features

- All constants are of type `BYTE` (range 0–255).
- The values are fixed and cannot be changed at runtime; they are compile‑time constants.
- The definition is encapsulated in a global constants type, making the addresses easily reusable across different projects.
- The comments provide a concise description of each source address, aiding documentation and code readability.
- The constants are defined in accordance with the ISO 11783 series, particularly parts 4, 10, and 14.

## State Overview

This global constant type does not maintain any internal state; it is a static collection of constant values. There are no state transitions or lifecycle operations. The constants are available immediately upon loading the type into a project.

## Application Scenarios

`IG2_Source_Addresses` is typically used in the development of ISOBUS‑based agricultural or forestry machinery, where multiple electronic control units (ECUs) need to communicate over the CAN bus. Examples include:

- Implementing a virtual terminal or display interface.
- Developing a tractor‑implement management (TIM) system.
- Configuring data loggers or task controllers.
- Setting up position control or header control systems.
- Assigning source addresses during network self‑configuration.

## Comparison with Similar Blocks

In the 4diac‑IDE environment, global constants are commonly used to define fixed configuration values. Similar constants might exist for other industry groups (e.g., IG1, IG3) or for other network‑specific parameters. The main advantage of this collection is its focus on IG2 addresses, providing a comprehensive and standardized set that directly corresponds to ISO 11783 recommendations. Compared to using individual magic numbers, these named constants improve code clarity and reduce the risk of address conflicts.

## Conclusion

The `IG2_Source_Addresses` global constant type offers a clean and standardized way to manage source addresses for ISO 11783 Industry Group 2 devices. By using these constants, developers can ensure compliance with international standards, simplify network configuration, and enhance the maintainability of their 4diac applications.