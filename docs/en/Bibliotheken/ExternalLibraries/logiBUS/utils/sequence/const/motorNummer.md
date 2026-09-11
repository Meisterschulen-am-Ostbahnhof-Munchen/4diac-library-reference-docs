# motorNummer

![motorNummer](./motorNummer.svg)

* * * * * * * * * *

## Introduction

The `motorNummer` global constant container provides a set of predefined constants that represent motor identities in an automation system. It is part of the `logiBUS::utils::sequence::const` package and is used in sequences such as `AnlagenSequenz_06.TRANSIT_MOTOR` to unambiguously identify which motor (or no motor) is currently selected. The constants are defined as 8-bit signed integers (`SINT`) and cover the range from "no motor" to six distinct motors.

## Interface Structure

Since `motorNummer` is a global constant container rather than a function block, it does not expose event or data interfaces in the traditional sense. Instead, it defines a fixed set of named constants that can be referenced globally throughout the project. The following sections describe the applicable interface elements.

### **Event Inputs**

Not applicable. The `motorNummer` container defines no event inputs.

### **Event Outputs**

Not applicable. The `motorNummer` container defines no event outputs.

### **Data Inputs**

Not applicable. The `motorNummer` container defines no modifiable data inputs; all values are read-only constants.

### **Data Outputs**

The container defines the following constant data values, each accessible by its symbolic name:

| Constant Name | Type | Value | Comment |
|---|---|---|---|
| `MOTOR_KEINER` | `SINT` | 0 | No motor selected |
| `MOTOR_M1` | `SINT` | 1 | Motor 1 |
| `MOTOR_M2` | `SINT` | 2 | Motor 2 |
| `MOTOR_M3` | `SINT` | 3 | Motor 3 |
| `MOTOR_M4` | `SINT` | 4 | Motor 4 |
| `MOTOR_M5` | `SINT` | 5 | Motor 5 |
| `MOTOR_M6` | `SINT` | 6 | Motor 6 |

### **Adapters**

Not applicable. The `motorNummer` container defines no adapters.

## Functionality

The primary purpose of `motorNummer` is to provide a centralized, human-readable naming scheme for motor identifiers. By using symbolic constants such as `MOTOR_M3` instead of raw numeric values, the application code becomes more readable, maintainable, and less error-prone. The constants are intended to be used in sequence logic, state machines, or any other part of the control application where a motor identity must be referenced. The special value `MOTOR_KEINER` (0) indicates that no motor is currently involved, enabling explicit handling of "no motor" states in sequences.

## Technical Features

- **Data Type**: All constants are of type `SINT` (8-bit signed integer), ensuring compact storage and compatibility with typical PLC data handling.
- **Fixed Values**: The constants are immutable and defined at compile time, preventing accidental modification during runtime.
- **Global Scope**: As global constants, they are accessible from any function block, subapplication, or adapter within the same project without requiring explicit data connections.
- **Package Organization**: The constants reside in the package `logiBUS::utils::sequence::const`, promoting a structured organization of project-wide constants.
- **Documentation Support**: Each constant includes a descriptive comment (e.g., "Motor 1", "no motor") to clarify its intended use.

## State Overview

Not applicable. `motorNummer` defines static constants and does not maintain any internal state or runtime behavior.

## Application Scenarios

The following scenarios are typical use cases for the `motorNummer` constants:

- **Motor Selection in Sequences**: In a production sequence (e.g., `AnlagenSequenz_06.TRANSIT_MOTOR`), the constants determine which motor is to be transported or activated. For example, checking `IF currentMotor = MOTOR_M2 THEN ...`.
- **Initialization**: Setting the motor reference to `MOTOR_KEINER` during system startup to indicate that no motor has been selected yet.
- **Switch/Case Logic**: Using the constants in case or switch statements to dispatch distinct behaviors for each motor.
- **Parameter Passing**: Passing a motor identifier to function blocks or subapplications as a clear, self-documenting argument.

## Comparison with Similar Blocks

Unlike function blocks or subapplications, `motorNummer` is a global constants container. It does not execute logic, maintain states, or process events. It is comparable to a named enum or define in traditional programming languages. A block like a motor control FB would use these constants as input parameters, while a subapplication might read them to configure its internal sequencing. The main advantage over hard-coded numeric values is the improved code readability and centralized definition, which simplifies maintenance when motor numbering changes.

## Conclusion

The `motorNummer` global constant container is a simple yet essential utility for standardizing motor identification in the automation project. By providing named, typed constants for all motor slots (and the "no motor" case), it enhances code clarity, reduces the risk of numeric errors, and supports consistent behavior across sequences and applications. It is a best-practice example of structuring project-wide constants for reuse.
