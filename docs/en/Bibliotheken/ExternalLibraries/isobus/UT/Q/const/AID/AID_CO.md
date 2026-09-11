# AID_CO

![AID_CO](./AID_CO.svg)

* * * * * * * * * *

## Introduction

AID_CO is a global constants definition block within the ISOBUS UT (Universal Terminal) interface library, specifically in the `isobus::UT::Q::const::AID` package. It defines constant attribute identifiers (IDs) used to reference attributes of container objects (CO) in ISOBUS object pool implementations. These attribute IDs conform to ISO 11783-6 (ISOBUS) standards for Universal Terminal object containers.

The constants map semantic meanings (e.g., width, height, hidden state) to numeric attribute IDs used in communication between a Virtual Terminal (VT) and an implement ECU.

## Interface Structure

This component is not a function block or adapter; it defines global compile-time constants. As such, it has no event inputs/outputs, no data I/O ports, and no adapters in the runtime sense. The constants act as named identifiers for use in application logic.

### **Event Inputs**

Not applicable – no event inputs are defined in this global constants container.

### **Event Outputs**

Not applicable – no event outputs are defined in this global constants container.

### **Data Inputs**

Not applicable – this global constants container does not accept external data inputs.

### **Data Outputs**

Not applicable – this global constants container does not provide dynamic data outputs. Instead, it exposes the following compile-time constants:

| Constant | Type   | Value | Attribute ID | Description |
|----------|--------|-------|--------------|-------------|
| `WIDTH`  | USINT  | 1     | AID_CO_WIDTH | Maximum width of the container's area in pixels. Objects or portions outside this area are clipped. |
| `HEIGHT` | USINT  | 2     | AID_CO_HEIGHT | Maximum height of the container's area in pixels. Objects or portions outside this area are clipped. |
| `HIDDEN` | USINT  | 3     | AID_CO_HIDDEN | 0 = FALSE, 1 = TRUE. Indicates whether this container and its child objects are hidden. |

### **Adapters**

Not applicable – no adapters are defined.

## Functionality

The AID_CO global constants provide numeric identifiers for the standard attributes of ISOBUS container objects. These constants are used when constructing attribute lists in object pool building or when setting/reading object attributes at runtime via the ISOBUS protocol. Using named constants instead of raw numeric values improves code readability and maintains compliance with the ISO 11783-6 specification.

- `WIDTH` (1) and `HEIGHT` (2) define the clipping boundary of the container's drawing area.
- `HIDDEN` (3) controls the visibility of the container and its child objects.

## Technical Features

- Defined in the `isobus::UT::Q::const::AID` package.
- All constants are of type `USINT` (Unsigned Short Integer) with explicit initialization.
- Values are aligned with the ISO 11783-6 attribute ID enumeration for container objects.
- Constant values are compile-time evaluated and cannot be changed at runtime.
- License: Eclipse Public License 2.0 (EPL-2.0).
- Version: 1.0, published 2026-06-20.

## State Overview

This component has no internal state machine or runtime states. It is a purely declarative constants container. All values are constant and available at compile time.

## Application Scenarios

- **ISOBUS Object Pool Generation:** Use `AID_CO.WIDTH`, `AID_CO.HEIGHT`, and `AID_CO.HIDDEN` when creating container object attributes in an object pool for a Virtual Terminal.
- **VT Runtime Communication:** Refer to these constants when setting or requesting container object attributes over the ISOBUS network.
- **Clipping and Visibility Control:** Apply the `HIDDEN` attribute (value 3) to show or hide containers dynamically, and `WIDTH`/`HEIGHT` to define the container's clipping rectangle.

## Comparison with Similar Blocks

In the ISOBUS `AID_*` constant families, `AID_CO` is responsible for container object attributes specifically. Similar constant groups include:

- `AID_OB` – object base attributes.
- `AID_WO` – working set attributes.
- `AID_OK` – object key attributes.

While those constants define attributes for other object types (e.g., buttons, keys, working sets), `AID_CO` focuses exclusively on container objects as defined in ISO 11783-6. Compared to generic numeric literals, using `AID_CO` constants increases readability and reduces the risk of using incorrect attribute IDs.

## Conclusion

The `AID_CO` global constants block provides a standardized, human-readable set of attribute identifiers for ISOBUS container objects. It is a lightweight, compile-time-only component that aids in implementing ISO 11783-6 compliant object pools and VT communication logic. Its use improves maintainability and ensures consistency across applications using the `isobus::UT::Q::const::AID` library.
