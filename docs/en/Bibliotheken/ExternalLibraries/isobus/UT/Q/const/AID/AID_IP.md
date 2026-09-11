# AID_IP

![AID_IP](./AID_IP.svg)

* * * * * * * * * *

## Introduction

The `AID_IP` global constants type defines a set of constant values used within the ISOBUS (ISO 11783) utility layer for handling input attributes. Specifically, it provides the validation type constant that controls how input attribute validation is performed. This type is part of the `isobus::UT::Q::const::AID` package and is intended to be included globally in IEC 61499 applications that deal with ISO‑bus device communication.

## Interface Structure

Since `AID_IP` is a **global constants** definition, it does not possess a traditional event/data input or output interface. Instead, it exposes a global constant that can be accessed by any function block or application component. The following sections list the available constant as part of the interface.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following global constant is defined:

| Constant Name | Data Type | Initial Value | Description |
|---------------|-----------|---------------|-------------|
| `VALIDATION_TYPE` | `USINT` | `USINT#1` | Specifies the validation type: `0` = valid characters are listed, `1` = invalid characters are listed. |

### **Adapters**

None.

## Functionality

The sole purpose of `AID_IP` is to provide a standardized, globally accessible constant for the validation mechanism of input attribute strings. By setting `VALIDATION_TYPE` to either `0` or `1`, the system can adapt its parsing and validation logic for input attributes. This constant is typically used in conjunction with other ISOBUS utility function blocks to ensure consistent behavior across an application.

## Technical Features

- **Single Global Constant**: Only one constant is defined, reducing complexity and potential runtime overhead.
- **USINT Data Type**: The constant is stored as an unsigned short integer, suitable for small numeric flags.
- **Pre‑initialized**: The constant is pre‑set to `1` (invalid‑character listing) at compile time, ensuring immediate usability without external configuration.
- **Namespace**: Belongs to the `isobus::UT::Q::const::AID` package, providing a clear hierarchical structure for related constants.

## State Overview

As a global constants type, `AID_IP` does not contain any state machines, execution states, or dynamic behavior. Its value is fixed and immutable during runtime.

## Application Scenarios

- **ISOBUS Input Validation**: When implementing the ISO 11783 utility layer, `VALIDATION_TYPE` can be used to switch between two validation modes for input attribute strings.
- **Configuration‑Free Systems**: Because the constant is pre‑defined, developers can rely on a consistent validation strategy without manual setup.
- **Multi‑Device Coordination**: Global constants help ensure that all parts of a distributed IEC 61499 application agree on the same validation rule.

## Comparison with Similar Blocks

Unlike function blocks (FBs) that have explicit event and data interfaces, `AID_IP` is purely a data definition. It is similar to global variable lists in other PLC programming environments but with the additional benefit of type‑safe, constant‑qualified definitions. FBs that require validation logic can read this constant directly, avoiding duplicate or conflicting definitions.

## Conclusion

The `AID_IP` global constants type fulfills a focused but essential role in ISOBUS‑based automation systems. By providing a single, well‑defined constant for validation type selection, it promotes code consistency and maintainability. Its simple structure and predetermined default value make it an unobtrusive yet valuable component for applications that handle ISO‑bus input attributes.
