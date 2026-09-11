# AID_NV

![AID_NV](./AID_NV.svg)

* * * * * * * * * *

## Introduction

This global constant definition provides a symbolic name for the attribute ID of a number variable object in the ISO 11783 (ISOBUS) protocol. It defines the constant `VALUE`, which is assigned the numeric value `1`, corresponding to the `AID_NV_VALUE` – the "current value" attribute of a number variable. The constant is intended for use in IEC 61499 applications that interact with ISOBUS object dictionaries, improving code clarity and maintainability.

## Interface Structure

As a global constants type, this element does not contain any event inputs, event outputs, data inputs, data outputs, or adapters. The following sections are therefore not applicable and are listed for completeness.

### **Event Inputs**
None (not applicable for global constants).

### **Event Outputs**
None (not applicable for global constants).

### **Data Inputs**
None (not applicable for global constants).

### **Data Outputs**
None (not applicable for global constants).

### **Adapters**
None (not applicable for global constants).

## Functionality

The primary purpose of this constant is to assign a human-readable identifier to the numeric attribute ID used in ISOBUS communication. Specifically, it defines the attribute ID for the "current value" of a number variable object. When developing IEC 61499 function blocks that read or write number variable values, this constant can be used to specify the target attribute without hard‑coding the raw numeric value.

## Technical Features

- **Type**: Global Constants (IEC 61499-1)
- **Constant Definition**:  
  `VALUE : USINT := 1`  (comment: `1: AID_NV_VALUE - Current value`)
- **Package/Namespace**: `isobus::UT::Q::const::AID` (set via `CompilerInfo`)
- **Standard Compliance**: IEC 61499-1
- **Version**: 1.0

## State Overview

Since this is a static constant definition, it does not possess any runtime state. The value is fixed at compile time and remains constant throughout the execution of the application.

## Application Scenarios

This constant is particularly useful in industrial automation applications that implement the ISOBUS protocol, such as agricultural machinery control systems. For example, when creating a function block to read the current numeric value from a tractor implement’s object dictionary, the constant `AID_NV.VALUE` can be passed as the attribute ID parameter, ensuring that the correct attribute is addressed.

## Comparison with Similar Blocks

In the ISOBUS context, global constants are defined for various object types (e.g., analog, digital, string) and their corresponding attributes. Compared to function blocks that handle data processing or communication, this constant is a static helper that simplifies code and reduces the risk of using incorrect attribute IDs. Similar constants would exist for other object attributes, each serving as a named reference to a specific numeric identifier.

## Conclusion

The `AID_NV` global constant provides a well‑defined, documented identifier for the "current value" attribute of number variable objects in ISOBUS. Its use promotes code readability, reduces maintenance overhead, and aligns with IEC 61499 best practices for separating constant definitions from application logic.