# AID_BO

![AID_BO](./AID_BO.svg)

* * * * * * * * * *
## Introduction

The `AID_BO` global constants type defines symbolic names for the attribute identifiers used to describe a button object in the ISOBUS (ISO 11783) standard. These identifiers are used when reading or writing button properties via the ISO‑11783 protocol, providing a clear and maintainable way to refer to numeric attribute codes in application code.

## Interface Structure

Since `AID_BO` is a global constants type, it does not contain any input or output variables in the traditional function‑block sense. Instead, it exposes a set of read‑only constant values that can be used throughout the application. These constants are listed below under **Data Outputs** for clarity, although they are not dynamic outputs.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following global constants are available within the `isobus::UT::Q::const::AID` package.

| Constant Name | Type  | Value (USINT) | Description |
|---------------|-------|---------------|-------------|
| `WIDTH`       | USINT | 1             | `AID_BO_WIDTH` – Width of the button in pixels. |
| `HEIGHT`      | USINT | 2             | `AID_BO_HEIGHT` – Height of the button in pixels. |
| `BACKGROUND_COLOUR` | USINT | 3       | `AID_BO_BACKGROUND_COLOUR` – Background colour index. |
| `BORDER_COLOUR`     | USINT | 4       | `AID_BO_BORDER_COLOUR` – Border colour index. |
| `KEY_CODE`          | USINT | 5       | `AID_BO_KEY_CODE` – Key code associated with the button. |
| `OPTIONS`           | USINT | 6       | `AID_BO_OPTIONS` – Bitmask containing several options:<br/>‑ Bit 0: latchable<br/>‑ Bit 1: state (0 = released, 1 = latched)<br/>‑ Bit 2: suppress border<br/>‑ Bit 3: transparent background<br/>‑ Bit 4: disabled<br/>‑ Bit 5: no border |

### **Adapters**

None.

## Functionality

The `AID_BO` constants serve as a centralised registry for button object attribute identifiers. In an ISOBUS application, object attributes are addressed by numeric IDs. Using symbolic constants improves code readability and reduces the risk of errors caused by hard‑coded numeric values. The package name `isobus::UT::Q::const::AID` indicates that these constants belong to the ISOBUS utility toolkit (`UT`) and are part of the `Q` – likely "query" or "quick" – subcomponent.

The `OPTIONS` constant is particularly important as it encapsulates a bit‑coded behaviour of the button. Applications can evaluate individual bits to determine the button’s properties (e.g., whether it is latchable, its current latched state, whether the border is suppressed, etc.).

## Technical Features

- **Data type**: All constants are of type `USINT` (unsigned short integer, 8‑bit).
- **Initial values**: Each constant is explicitly initialised to its corresponding attribute ID.
- **Package**: `isobus::UT::Q::const::AID` – the global constants are grouped under this package, allowing namespaces to be used in IEC 61499 applications.
- **Compatibility**: The identifiers follow the official ISO 11783‑6 (ISOBUS) attribute definitions, ensuring interoperability with other ISOBUS‑compliant devices.
- **Source availability**: The original source code is embedded in the XML, making the definitions self‑contained and accessible for reference.

## State Overview

The `AID_BO` type has no dynamic state information. It is a static collection of constants that do not change during runtime. The values are fixed at compile time and remain unchanged throughout the execution of the application.

## Application Scenarios

Typical use cases for `AID_BO` include:

- **Implementation of an ISOBUS‑VT (Virtual Terminal) client**: When constructing or updating a button object on the VT, the application needs to send attribute IDs for width, height, background colour, etc. Using `AID_BO` constants makes the code self‑documenting and less prone to mistakes.
- **Parsing feedback messages**: When the VT sends back the current state of a button (e.g., latched or released), the application can interpret the `OPTIONS` value by referencing the bits defined in this constant.
- **Maintenance and debugging**: Developers can trace protocol messages more easily when symbolic names are used in log output.

## Comparison with Similar Blocks

While `AID_BO` is a global constants type, other similar constant types may exist for other ISOBUS objects (e.g., `AID_AL` for alarm objects, `AID_NM` for numerical display, etc.). These constants typically follow the same pattern – each object type has a dedicated set of attribute identifiers defined as USINT constants. The advantage of such symbolic constants over raw numbers is consistency and clarity across a project. Compared to function blocks, `AID_BO` does not contain any logic or user interface; it is purely a data dictionary.

## Conclusion

The `AID_BO` global constants provide a well‑structured and standardised way to reference button object attributes in an ISOBUS application. By using these symbolic names, developers can write more reliable and maintainable code, reduce the chance of encoding errors, and improve overall readability of IEC 61499 applications that interface with ISOBUS virtual terminals. The explicit packaging under `isobus::UT::Q::const::AID` further enhances modularity and integration into larger projects.