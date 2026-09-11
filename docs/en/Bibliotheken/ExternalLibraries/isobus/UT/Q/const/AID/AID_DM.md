# AID_DM

![AID_DM](./AID_DM.svg)

* * * * * * * * * *

## Introduction

AID_DM is a global constant definition used within 4diac IDE to define attribute identifiers for data mask objects in the ISOBUS protocol. It provides symbolic constants for background colour and soft key mask attributes, promoting code readability and maintainability in ISOBUS-based applications.

## Interface Structure

### **Event Inputs**

None applicable – this is a global constant definition, not a function block.

### **Event Outputs**

None applicable.

### **Data Inputs**

None applicable.

### **Data Outputs**

None applicable.

### **Adapters**

None applicable.

## Functionality

This global constant set defines two `USINT` constants:

- `BACKGROUND_COLOUR` (value 1): corresponds to the attribute ID `AID_DM_BACKGROUND_COLOUR`, used to specify the background colour index of a data mask.
- `SOFT_KEY_MASK` (value 2): corresponds to the attribute ID `AID_DM_SOFT_KEY_MASK`, used to specify the object ID of a Soft Key Mask associated with the data mask.

These constants are intended to be used in ISOBUS-based applications to reference these attribute IDs symbolically, improving clarity and reducing hard-coded numeric values.

## Technical Features

- Defined in package `isobus::UT::Q::const::AID`.
- Uses data type `USINT` (unsigned short integer).
- Constant values are preinitialized: 1 and 2.
- The constants are global and read-only within the 4diac environment.

## State Overview

Not applicable – this definition does not contain states or state machines; it is a static data definition.

## Application Scenarios

- Developing ISOBUS-compliant applications that require referencing Data Mask attributes.
- Used in conjunction with ISOBUS object pool management to set or query background colour and soft key mask attributes.
- Provides a centralized and standardised way to access these attribute identifiers in 4diac projects.

## Comparison with Similar Blocks

Since this is a global constant definition, it is similar to other constant collections in 4diac that define attribute or parameter IDs. For example, other `AID_*` global constants might define attribute IDs for different ISOBUS objects. This particular set focuses solely on Data Mask attributes, making it lightweight and specific.

## Conclusion

AID_DM is a simple yet essential global constant definition for ISOBUS data mask attribute management. By providing symbolic names for the background colour and soft key mask attribute IDs, it enhances code clarity and reduces the risk of hard-coded numbers. It is a foundational building block for ISOBUS applications within 4diac.
