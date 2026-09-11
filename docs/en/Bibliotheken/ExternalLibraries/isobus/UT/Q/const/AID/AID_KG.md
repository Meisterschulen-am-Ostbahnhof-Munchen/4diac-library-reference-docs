# AID_KG

![AID_KG](./AID_KG.svg)

* * * * * * * * * *
## Introduction

The `AID_KG` global constant set defines standardized attribute identifiers for ISO‑bus key group objects. It is part of the `isobus::UT::Q::const` package and provides fixed numeric values that unambiguously reference specific object attributes, such as availability options and naming information. These constants simplify integration with ISO‑bus communication stacks and ensure consistent code across applications.

## Interface Structure

### **Event Inputs**
None – this is a global constant definition, not a function block with event inputs.

### **Event Outputs**
None – no event outputs are provided.

### **Data Inputs**
None – the block does not accept any variable inputs; all values are instantiated as compile‑time constants.

### **Data Outputs**
None – the constants are accessible globally by their symbolic names (e.g., `OPTIONS`, `NAME`) and are not exchanged via formal output connections.

### **Adapters**
None – no adapter interfaces are defined.

## Functionality

The purpose of `AID_KG` is to centralize and standardize the numeric identifiers used for key group object attributes in ISO‑bus systems. It declares two constants:

| Constant Name | Type   | Value | Description                                                                 |
|---------------|--------|-------|-----------------------------------------------------------------------------|
| `OPTIONS`     | `USINT`| 1     | Bitmask that defines attribute availability: Bit 0 = Available, Bit 1 = Transparent. |
| `NAME`        | `USINT`| 2     | Object ID of an output string or object pointer containing the attribute name. |

These constants are used in conjunction with ISO‑bus protocol operations to reference specific attributes without hard‑coding numeric literals, improving readability and maintainability of code.

## Technical Features

- **Compile‑time constants**: Values are defined as `GLOBALCONSTANTS` in the IEC 61499 environment, ensuring that they are immutable and available throughout the application.
- **Type safety**: All constants are declared as `USINT` (unsigned short integer) to ensure compatibility with ISO‑bus attribute fields.
- **Package organization**: The constants reside in the `isobus::UT::Q::const::AID` package, allowing for structured inclusion and avoiding name conflicts.
- **Simple integration**: The constants can be directly referenced in function blocks, algorithms, or other parts of the application without additional configuration.

## State Overview

Since `AID_KG` defines static constants, it does not possess any runtime state. The values are fixed at compile time and do not change during execution. Therefore, there is no state machine, no initialization, and no internal variables to manage.

## Application Scenarios

- **ISO‑bus object modeling**: When implementing object dictionaries or key groups, `OPTIONS` and `NAME` are used to set or query attribute flags and naming data.
- **Protocol‑related function blocks**: Blocks that interact with ISO‑bus objects can use these constants to identify attributes during read/write operations.
- **Cross‑component consistency**: By referencing the same constants in multiple FBs, the risk of attribute ID mismatches is eliminated, simplifying maintenance and diagnosis.

## Comparison with Similar Blocks

- **vs. local constants**: Unlike local constants within an FB, `AID_KG` is globally accessible, avoiding duplication across multiple FBs and ensuring a single source of truth.
- **vs. enumeration types**: Enumerations provide symbolic names but are not necessarily bound to numeric values; `AID_KG` explicitly maps symbolic names to fixed integer IDs, which is required for wire‑level protocol compatibility.
- **vs. configuration parameters**: Unlike configurable parameters, these constants are pre‑defined and cannot be altered at runtime, which is appropriate for protocol‑fixed identifiers.

## Conclusion

`AID_KG` is a lightweight, standardized global constant set that defines essential attribute identifiers for ISO‑bus key group objects. Its use promotes code clarity, reduces errors, and supports portable, maintainable applications within the 4diac‑IDE environment. Though it contains no dynamic logic, it plays a fundamental role in ensuring consistent object attribute handling across a distributed system.