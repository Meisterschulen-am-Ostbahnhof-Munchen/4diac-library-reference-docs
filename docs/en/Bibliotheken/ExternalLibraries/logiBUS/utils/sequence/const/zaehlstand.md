# zaehlstand

![zaehlstand](./zaehlstand.svg)

* * * * * * * * * *

## Introduction

The `zaehlstand` global constants block defines a set of named constants representing the current number of running motors in an industrial plant sequence. The constants are of type `SINT` (signed 8‑bit integer) and cover the range from 0 to 6 motors. The constant names are used for readability and maintainability, replacing hard‑coded numeric values in application logic. The block is part of the `logiBUS::utils::sequence::const` package and is intended for use within the 4diac IDE environment.

## Interface Structure

The `zaehlstand` block does not contain any executable function block logic – it is a pure constant definition. Therefore, it exposes no event inputs, event outputs, or adapters. The provided values are accessible as global constants within the application.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following constants are defined as global data outputs. They hold fixed values and cannot be modified at runtime.

| Constant Name   | Type | Initial Value | Description                               |
|-----------------|------|---------------|-------------------------------------------|
| `ZAEHLSTAND_0`  | SINT | `SINT#0`      | 0 motors running (state S_AUS)            |
| `ZAEHLSTAND_1`  | SINT | `SINT#1`      | 1 motor running                           |
| `ZAEHLSTAND_2`  | SINT | `SINT#2`      | 2 motors running                          |
| `ZAEHLSTAND_3`  | SINT | `SINT#3`      | 3 motors running                          |
| `ZAEHLSTAND_4`  | SINT | `SINT#4`      | 4 motors running                          |
| `ZAEHLSTAND_5`  | SINT | `SINT#5`      | 5 motors running                          |
| `ZAEHLSTAND_6`  | SINT | `SINT#6`      | 6 motors running (state S_LAEUFT, all)    |

### **Adapters**

None.

## Functionality

The block provides a set of symbolic constants that represent the exact number of motors currently active in a plant sequence. By referencing these constants in the application code, the developer can easily compare the current motor count (e.g., from a counter output) with a specific scenario. The constants are defined in a single location, simplifying changes and avoiding magic numbers scattered throughout the project.

## Technical Features

- **Type:** All constants are of type `SINT` (signed 8‑bit integer).
- **Initial Values:** Each constant is pre‑initialised with the corresponding numeric value (`SINT#0` to `SINT#6`).
- **Scope:** Defined as `VAR_GLOBAL CONSTANT` – they are globally accessible and immutable.
- **Package:** Belongs to the `logiBUS::utils::sequence::const` package, which groups related sequence constants.
- **Licensing:** Distributed under Eclipse Public License 2.0.

## State Overview

The constants represent discrete states of the motor count:

- `ZAEHLSTAND_0` – all motors off (S_AUS)
- `ZAEHLSTAND_6` – all motors running (S_LAEUFT)
- Intermediate values represent partially active motor sets.

These constants are typically used in sequence control logic to trigger actions or transitions when a certain motor count is reached.

## Application Scenarios

- **Sequence Control:** Determine when to start or stop additional motors based on the current count.
- **Monitoring:** Compare the actual motor count (e.g., from a sensor or counter) with the expected count.
- **Interlocking:** Enable or disable processes when the number of running motors reaches a critical threshold.
- **Testing:** Use the constants as test inputs for simulation or validation of motor‑counting logic.

## Comparison with Similar Blocks

Unlike function blocks (FBs) or adapters, `zaehlstand` is a **constant definition** – it does not perform any computation or communication. It serves a similar purpose as an enumeration, but with explicit numeric values. Compared to using raw integer literals, the constants improve code readability and reduce the risk of typographical errors. Other possible implementations might use an enumerated type or a structured variable, but the use of simple SINT constants keeps the approach lightweight and directly compatible with IEC 61499 arithmetic operations.

## Conclusion

`zaehlstand` is a compact and well‑defined set of global constants for representing the number of running motors in a plant sequence. It provides clear, mnemonic names for the values 0 through 6, promoting maintainable and error‑free application code. The block is an essential building block for sequence logic within the 4diac environment.
