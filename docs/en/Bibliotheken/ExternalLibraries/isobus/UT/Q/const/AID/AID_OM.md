# AID_OM

![AID_OM](./AID_OM.svg)

* * * * * * * * * *
## Introduction

`AID_OM` is a global constant definition set used in the context of ISOBUS virtual terminal (VT) applications. It defines attribute identifiers (IDs) for the **Output Meter** object, which is a graphical element used to display a value on a scale, typically with a needle and tick marks. The constants provide a standardized, human-readable way to reference the attributes of an output meter when constructing or modifying VT object data.

This global constant set belongs to the package `isobus::UT::Q::const::AID` and contains twelve `USINT` (unsigned short integer) constants, each representing a specific attribute ID (1 to 12) of the output meter object according to the ISOBUS standard ISO 11783.

## Interface Structure

Since `AID_OM` is a global constants definition rather than a function block, adapter, or subapplication, it does not expose any runtime interface. The following sections reflect the standard FB interface structure and are marked "Not applicable" where no corresponding members exist. The actual constant definitions are listed in the **Global Constants** subsection.

### **Event Inputs**

Not applicable – `AID_OM` defines no event inputs.

### **Event Outputs**

Not applicable – `AID_OM` defines no event outputs.

### **Data Inputs**

Not applicable – `AID_OM` defines no data inputs, as it is a set of compile-time constants.

### **Data Outputs**

Not applicable – `AID_OM` defines no data outputs.

### **Adapters**

Not applicable – `AID_OM` contains no adapter instances.

### **Global Constants**

The following table lists all constants defined in `AID_OM`. Each constant holds the numeric attribute ID used for the corresponding output meter property in a VT object.

| Constant Name   | Type   | Value | Attribute ID | Description |
|-----------------|--------|-------|--------------|-------------|
| `WIDTH`         | USINT  | 1     | 1            | Width in pixels (AID_OM_WIDTH). |
| `NEEDLE_COLOUR` | USINT  | 2     | 2            | Needle colour index (AID_OM_NEEDLE_COLOUR). |
| `BORDER_COLOUR` | USINT  | 3     | 3            | Border colour index (AID_OM_BORDER_COLOUR). |
| `ARC_THICK_COLOUR` | USINT | 4  | 4            | Arc thickness / colour index (AID_OM_ARC_THICK_COLOUR). |
| `OPTIONS`       | USINT  | 5     | 5            | Bitmask options: Bit 0 = Draw Arc, Bit 1 = Draw Border, Bit 2 = Draw Ticks, Bit 3 = Deflection direction (0 = anticlockwise, 1 = clockwise). |
| `NUMB_TICKS`    | USINT  | 6     | 6            | Number of tick marks (AID_OM_NUMB_TICKS). |
| `START_ANGLE`   | USINT  | 7     | 7            | Start angle of the scale (AID_OM_START_ANGLE). |
| `END_ANGLE`     | USINT  | 8     | 8            | End angle of the scale (AID_OM_END_ANGLE). |
| `MIN_VALUE`     | USINT  | 9     | 9            | Minimum displayed value (AID_OM_MIN_VALUE). |
| `MAX_VALUE`     | USINT  | 10    | 10           | Maximum displayed value (AID_OM_MAX_VALUE). |
| `VARIABLE_REF`  | USINT  | 11    | 11           | Object ID of a Variable object (AID_OM_VARIABLE_REF). |
| `VALUE`         | USINT  | 12    | 12           | Current value shown by the meter (AID_OM_VALUE). |

## Functionality

The `AID_OM` constants serve as static symbolic references for the attribute identifiers of an ISOBUS output meter object. When a control application needs to create, modify, or read an output meter object on a VT, it uses these constants to specify which attribute is being addressed. For example, setting the `OPTIONS` attribute to a desired bitmask requires referring to it as `AID_OM.OPTIONS` rather than using the raw numeric ID `5`.

Because the constants are defined once in a central location, they improve code readability, reduce the risk of typographical errors, and simplify maintenance if attribute IDs ever change in the future.

## Technical Features

- **Type**: All constants are of type `USINT` (8-bit unsigned integer).
- **Initial Values**: Each constant is explicitly initialized to its attribute ID value (1 to 12) using typed literals (e.g., `USINT#1`).
- **Package**: `isobus::UT::Q::const::AID` – indicates the constant set belongs to the ISOBUS utility toolkit, within the object-oriented VT class hierarchy.
- **Standard Compliance**: The constants follow the attribute ID numbering defined by ISO 11783 (ISOBUS) for the Output Meter object.
- **No Runtime Side Effects**: As a `GlobalConstants` definition, it only influences compilation and does not occupy runtime memory or trigger logic execution.

## State Overview

Not applicable – `AID_OM` contains no state information, no state machines, and no execution logic. It is a purely declarative constant set.

## Application Scenarios

- **VT Object Construction**: When building an output meter object via a control function, use the constants to populate attribute fields in a structured way.
- **Dynamic Update**: When updating the meter's current value (`VALUE`), the constant can be used to set the corresponding attribute in a VT command without hard-coding numeric IDs.
- **Configuration**: Use `OPTIONS` to configure the visual appearance (arc, border, ticks, deflection direction) of the meter.
- **Scaling**: Use `MIN_VALUE`, `MAX_VALUE`, `START_ANGLE`, and `END_ANGLE` to define the scale's range and angular span.

## Comparison with Similar Blocks

`AID_OM` is one of several global constant definitions under the `isobus::UT::Q::const::AID` package. Similar sets exist for other VT object types (e.g., output line, output rectangle, etc.), each following the same pattern: a collection of `USINT` constants with values matching the standard attribute IDs. The advantage of this approach over raw numeric literals is consistent naming, centralized documentation, and compile-time type safety. In contrast to function blocks or subapplications, `AID_OM` provides no algorithmic behavior – it serves purely as a reference library for application developers.

## Conclusion

The `AID_OM` global constants set provides a clean, standardized, and type-safe means of referencing output meter attribute IDs in ISOBUS VT applications. By using these symbolic constants instead of raw numbers, developers can write more readable, maintainable, and error-resistant code when working with output meter objects.