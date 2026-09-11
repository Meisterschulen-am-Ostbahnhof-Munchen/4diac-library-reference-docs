# AID_SK

![AID_SK](./AID_SK.svg)

* * * * * * * * * *

## Introduction

AID_SK is a global constant container that defines attribute identifiers (IDs) for soft key objects in an ISOBUS Virtual Terminal (VT) environment. It provides standardized numeric constants used to reference specific properties of soft keys during communication between the control function and the VT.

The constants are defined as unsigned 8-bit integers (USINT) and are part of the `isobus::UT::Q::const::AID` package. They serve as a central source of truth for soft key attribute IDs, ensuring consistency across applications and reducing the risk of hard-coded values.

## Interface Structure

AID_SK is a global constant definition and does not expose any event interfaces, data inputs/outputs, or adapters. It contains only predefined constant values.

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

The purpose of AID_SK is to provide two constant values that correspond to the attribute IDs used in ISOBUS VT soft key objects:

- `BACKGROUND_COLOUR` (value = 1): This ID refers to the background colour index of a soft key.
- `KEY_CODE` (value = 2): This ID is used by the VT to report the code in a **Soft Key Activation** message.

By defining these values as global constants, all parts of an application can reference them by name rather than by numeric literals, improving code readability and maintainability.

## Technical Features

- **Constants**: Both constants are of type `USINT` (unsigned 8-bit integer).
- **Initial Values**: Predefined fixed values: `BACKGROUND_COLOUR = 1`, `KEY_CODE = 2`.
- **Package**: The constants are encapsulated in the `isobus::UT::Q::const::AID` package, following the ISOBUS naming conventions.
- **Read-Only**: These are global constants, meaning they cannot be modified at runtime.

## State Overview

Not applicable – AID_SK is a static constant container and has no runtime state or behavior.

## Application Scenarios

AID_SK is typically used in ISOBUS VT applications, particularly when implementing soft key interactions. For example:

- When configuring a soft key’s background colour, the application can use `AID_SK.BACKGROUND_COLOUR` to refer to the corresponding attribute ID.
- When handling a **Soft Key Activation** event, the VT sends a key code that matches `AID_SK.KEY_CODE`. Applications can compare or utilize this value to identify which soft key was activated.

This constant container helps standardize attribute IDs across different parts of an application and aligns with the ISOBUS standard’s definitions.

## Comparison with Similar Blocks

In the same package `isobus::UT::Q::const::AID`, other constant containers exist for different object types (e.g., `AID_OK` for object keys, `AID_AT` for attributes, etc.). AID_SK specifically focuses on soft key attributes. Compared to other constants, AID_SK defines only two IDs that are directly related to soft keys, making it a minimal and focused constant set.

## Conclusion

AID_SK provides two essential constant values for soft key attribute IDs in ISOBUS VT applications. By centralizing these definitions, it promotes code clarity, reduces errors, and aligns with standard ISOBUS conventions. The constant container is simple, yet crucial for any application involving soft key configuration or activation handling.
