# AID_SG

![AID_SG](./AID_SG.svg)

* * * * * * * * * *
## Introduction

The `AID_SG` global constant set defines attribute identifiers for scaled graphic objects in the ISOBUS Universal Terminal (UT) protocol. These constants are used to reference specific properties of scaled graphical elements, such as width, height, scaling behavior, options, and value, within a 4diac‑based control application. They are part of the `isobus::UT::Q::const::AID` package and are intended to improve code readability and maintainability by providing symbolic names for numeric attribute IDs.

## Interface Structure

As a set of global constants, `AID_SG` does not possess an event or adapter interface. The following sections describe the available data constants.

### **Event Inputs**
None.

### **Event Outputs**
None.

### **Data Inputs**
The following constants are defined globally and can be used directly within function blocks or programs:

| Name        | Type   | Value   | Description                                                                                 |
|-------------|--------|---------|---------------------------------------------------------------------------------------------|
| `WIDTH`     | `USINT`| `USINT#1` | `AID_SG_WIDTH` – Width in pixels.                                                          |
| `HEIGHT`    | `USINT`| `USINT#2` | `AID_SG_HEIGHT` – Height in pixels.                                                        |
| `SCALE_TYPE`| `USINT`| `USINT#3` | `AID_SG_SCALE_TYPE` – Scaling and justification encoding (see bit field description below).|
| `OPTIONS`   | `USINT`| `USINT#4` | `AID_SG_OPTIONS` – Bitmask; Bit 0 = Flashing.                                              |
| `VALUE`     | `USINT`| `USINT#5` | `AID_SG_VALUE` – Current value of the scaled graphic object.                                |

### **Data Outputs**
None.

### **Adapters**
None.

## Functionality

The constants in `AID_SG` provide standardized identifiers for the attributes of scaled graphic objects as defined by the ISOBUS standard. The `SCALE_TYPE` constant encapsulates both scaling behaviour (bits 0–2) and horizontal/vertical justification (bits 3–6). The `OPTIONS` constant is used to enable additional visual effects, such as flashing. These constants are intended to be used in conjunction with ISOBUS UT services that read or write object properties – they map directly to the attribute ID numbers defined in the protocol.

## Technical Features

- All constants are of type `USINT` (unsigned short integer), occupying one byte.
- Values are fixed at compile time and cannot be changed during runtime.
- The `SCALE_TYPE` field is a packed bit field:
  - Bits 0–2: Scaling value – `0` = no scale, `1` = scale to width, `2` = scale to height, `3` = scale to width and height, `4` = scale to fit.
  - Bits 3–4: Horizontal justification – `0` = left, `1` = middle, `2` = right.
  - Bits 5–6: Vertical justification – `0` = top, `1` = middle, `2` = bottom.
- The `OPTIONS` constant currently defines only bit 0 (flashing); remaining bits are reserved for future use.

## State Overview

Not applicable – global constants do not have state.

## Application Scenarios

These constants are typically used in ISOBUS UT applications where scaled graphic objects (e.g., gauges, bars, or image containers) need to be configured or dynamically updated. For example:

- Setting the scaling parameters of a graphic element using `SCALE_TYPE`.
- Reading or writing the `VALUE` attribute to update the displayed value.
- Enabling or disabling flashing via the `OPTIONS` bitmask.

By referencing these constants, developers avoid hard‑coding numeric attribute IDs, reducing the risk of errors and improving code clarity.

## Comparison with Similar Blocks

Other global constant sets exist for different ISOBUS object types, such as `AID_OBJ` for general objects or `AID_TXT` for text objects. `AID_SG` is specifically tailored to scaled graphic objects and provides only the attributes relevant to that class. Unlike an FB, it has no executable logic; it serves purely as a lookup table for attribute identifiers.

## Conclusion

The `AID_SG` global constant set is a lightweight but essential component for ISOBUS UT development in 4diac. It encapsulates the numeric attribute IDs for scaled graphic objects in an easy‑to‑use symbolic form, supporting both readability and portability. Its fixed definitions align with the ISOBUS standard, making it a reliable building block for agricultural control systems.