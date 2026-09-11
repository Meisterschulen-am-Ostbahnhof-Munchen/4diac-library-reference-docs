# AID_OR

![AID_OR](./AID_OR.svg)

* * * * * * * * * *

## Introduction

The `AID_OR` global constants block defines a set of attribute identifiers used for output rectangle objects in the ISOBUS (ISO 11783) agricultural electronics protocol. These constants provide a standardized way to reference the attributes of an output rectangle, such as line style, width, height, line suppression, and fill attributes. The values are defined as unsigned short integers (USINT) and are intended to be used in conjunction with object pool definitions and communication interfaces.

## Interface Structure

The `AID_OR` block does not contain any event inputs, event outputs, or adapters. It solely provides a collection of data constants that can be referenced globally throughout a 4diac application.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following table lists the constant data values defined by this block. They are accessible as read-only variables.

| Name         | Type  | Value | Comment                                                                       |
|--------------|-------|-------|-------------------------------------------------------------------------------|
| `LINE_ATT`   | USINT | 1     | Object ID of a Line Attributes object.                                       |
| `WIDTH`      | USINT | 2     | Width in pixels.                                                             |
| `HEIGHT`     | USINT | 3     | Height in pixels.                                                            |
| `LINE_SUPPR` | USINT | 4     | Line suppression attribute.                                                  |
| `FILL_ATT`   | USINT | 5     | Object ID of a Fill Attributes object.                                       |

### **Adapters**

None.

## Functionality

The global constants are designed to be used as arguments when creating or modifying output rectangle objects in an ISOBUS object pool. By referencing these named constants instead of hard-coded numeric values, the application code becomes more readable, self-documenting, and less error-prone. For example, when setting the width attribute of a rectangle, the constant `AID_OR.WIDTH` can be used to indicate that the following value refers to the width in pixels.

## Technical Features

- **Type:** All constants are of type `USINT` (unsigned short integer), capable of representing values from 0 to 255.
- **Package:** The constants belong to the package `isobus::UT::Q::const::AID`, which organizes them within the namespace hierarchy.
- **Initialization:** Each constant is initialized with a specific attribute ID, ensuring consistent behavior across all referencing components.
- **Read-Only:** These are global constants and cannot be modified at runtime, providing compile-time safety.

## State Overview

Since `AID_OR` contains only constants, there is no runtime state or dynamic behavior. The values are fixed at compile time and remain constant throughout the execution of the application.

## Application Scenarios

- **Object Pool Creation:** When constructing an ISOBUS object pool, the developer can use these constants to define properties of output rectangle objects, such as line type, dimensions, or fill style.
- **Communication Parameterization:** The constants can be passed to commands that update the attributes of a rectangle object on a virtual terminal or control unit.
- **Code Maintainability:** Using named constants improves code clarity and makes it easier to update attribute IDs if the ISOBUS specification changes, since only the constant definitions need to be modified.

## Comparison with Similar Blocks

In the same package, there may be similar global constant blocks for other object types (e.g., `AID_OT` for output text, `AID_OB` for output bar). `AID_OR` specifically focuses on output rectangle attributes, providing a distinct set of IDs tailored to that object type. Unlike function blocks that encapsulate logic, `AID_OR` is a pure data definition unit, making it lightweight and universally applicable.

## Conclusion

The `AID_OR` global constants block is an essential utility for any ISOBUS-based application that deals with output rectangle objects. It centralizes attribute identifiers, promotes code reuse, and ensures consistency across different modules. By adopting these constants, developers can build more robust and maintainable agricultural automation systems.
