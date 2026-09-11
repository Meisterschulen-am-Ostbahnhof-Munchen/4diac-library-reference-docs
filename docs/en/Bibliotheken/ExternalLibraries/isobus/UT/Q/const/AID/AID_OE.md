# AID_OE

![AID_OE](./AID_OE.svg)

* * * * * * * * * *

## Introduction

The **AID_OE** global constant group defines the attribute identifiers (AIDs) for **Output Ellipse** objects within the ISOBUS (ISO 11783) object pool. It is part of the `isobus::UT::Q::const::AID` package and provides a standardized set of constant values used to reference specific attributes of an ellipse object (e.g., line style, size, type, angles, fill properties). These constants are essential for correctly reading and writing object attributes when configuring or updating an ellipse on a Virtual Terminal (VT) in agricultural machinery.

## Interface Structure

This global constant group does not possess an event or data interface typical of function blocks. Instead, it exposes a set of read-only constant values that can be used globally within the IEC 61499 application.

### **Event Inputs**

*None.*

### **Event Outputs**

*None.*

### **Data Inputs**

*None.*

### **Data Outputs**

*None (the constants are global, not interface-specific).*

### **Adapters**

*None.*

## Functionality

The `AID_OE` constants represent the numeric attribute identifiers for an output ellipse object as defined by the ISOBUS standard. Each constant corresponds to a specific attribute that can be queried or modified via the object pool services. The values are fixed and immutable, ensuring consistent communication between the VT and the implement controller.

The constants and their meanings are:

| Constant Name | Value | Description |
|---------------|-------|-------------|
| `LINE_ATT`    | 1     | Object ID of a Line Attributes object. |
| `WIDTH`       | 2     | Width in pixels. |
| `HEIGHT`      | 3     | Height in pixels. |
| `ELLIPSE_TYPE`| 4     | Ellipse type: 0=Closed Ellipse, 1=Open Ellipse, 2=Closed Segment, 3=Closed Section. |
| `START_ANGLE` | 5     | Start angle of the ellipse (in degrees). |
| `END_ANGLE`   | 6     | End angle of the ellipse (in degrees). |
| `FILL_ATT`    | 7     | Object ID of a Fill Attributes object. |

## Technical Features

- **Data Type**: All constants are of type `USINT` (unsigned short integer, 8-bit).
- **Fixed Values**: The values are pre-assigned and cannot be changed at runtime.
- **Global Scope**: The constants are accessible from any FB or resource within the 4diac project, promoting code readability and maintainability.
- **Standard Compliance**: They follow the ISOBUS 11783 object pool attribute definitions for output ellipses.
- **Package Location**: Defined within the `isobus::UT::Q::const::AID` namespace, grouping related attribute constants.

## State Overview

This global constant group has no state information. It is a static definition file – its values remain unchanged throughout the application lifetime. There are no operational states, transitions, or internal logic.

## Application Scenarios

- **Object Pool Creation**: When building an ISOBUS object pool that includes ellipse objects, these constants are used to set or retrieve specific ellipse properties such as width, height, or type.
- **Dynamic Attribute Updates**: In a running VT application, if the implement controller needs to modify an ellipse’s dimensions or appearance, it uses these AIDs as command identifiers in the `Change Object Attribute` service.
- **Consistency Across Projects**: By referencing these constants instead of hard-coded numbers, developers avoid errors and improve code clarity.
- **Interoperability**: Ensures that all ellipse-related attributes use standardized identifiers, facilitating communication between equipment from different manufacturers.

## Comparison with Similar Blocks

Unlike function blocks that process data or events, `AID_OE` is a passive constant container. It does not execute logic, consume CPU time, or require instantiation. In comparison:

- **Function Blocks** (e.g., `ELLIPSE_CTRL`) actively manage and update ellipse objects, often using these AIDs internally.
- **Other Global Constant Groups** (e.g., `AID_OL` for output line objects) serve the same purpose but for different object types. `AID_OE` specifically targets ellipses, ensuring accurate attribute mapping.
- **Constants vs. Variables**: Constants are immutable, whereas variables can change. Using constants helps prevent accidental modification of critical attribute identifiers.

## Conclusion

The `AID_OE` global constant group provides a reliable, standardised set of attribute identifiers for ISOBUS output ellipse objects. It simplifies object pool management, enhances code maintainability, and ensures compliance with international agricultural communication standards. By leveraging these constants, developers can focus on application logic without worrying about numeric identifier collisions or inconsistencies.
