# IG0_Device_Classes

![IG0_Device_Classes](./IG0_Device_Classes.svg)

* * * * * * * * * *

## Introduction

This global constants block defines industry group 0 specific device classes for vehicle systems, according to ISO 11783 (ISOBUS) PGN constants. It provides two standard constants to identify device classes for non‑specific systems and unavailable systems.

## Interface Structure

This element is a global constants definition, not a function block; therefore, it has no event or data inputs/outputs or adapters.

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

The constants provide standard values for device class identification in vehicle systems. They are used to indicate that a device class is non‑specific (value 0) or not available (value 127). These constants are intended to be referenced in other parts of the system to avoid magic numbers and ensure consistency.

## Technical Features

- **Data Type:** BYTE (8‑bit unsigned integer)  
- **Constants:**
  - `DC_NON_SPECIFIC_SYSTEM` = 0  
  - `DC_NOT_AVAILABLE` = 127  
- Defined as global constants, accessible across the project.  
- Part of the `isobus::pgn::const` package.

## State Overview

Not applicable; constants do not have states.

## Application Scenarios

- Use `DC_NON_SPECIFIC_SYSTEM` to indicate that a device class is not specifically defined or is a generic system.  
- Use `DC_NOT_AVAILABLE` to indicate that the device class information is not available or unknown.  
- These constants can be used in PGN (Parameter Group Number) processing for ISOBUS compliant devices.

## Comparison with Similar Blocks

This is a constants definition; typical alternatives might be direct literals, but using named constants improves maintainability and readability. Similar constants may exist for other industry groups; this one focuses on vehicle systems.

## Conclusion

`IG0_Device_Classes` provides a standardized set of device class constants for ISOBUS vehicle systems, ensuring consistent and self‑documenting code.
