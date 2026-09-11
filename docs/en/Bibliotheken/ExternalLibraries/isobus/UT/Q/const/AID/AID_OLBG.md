# AID_OLBG

![AID_OLBG](./AID_OLBG.svg)

* * * * * * * * * *

## Introduction

The `AID_OLBG` Global Constants definition provides a structured set of named constants that identify attribute IDs for an **Output Linear Bar Graph** object in ISOBUS-based communication systems (ISO 11783). These constants serve as a lookup table for object attribute identifiers used when configuring or updating linear bar graph elements on virtual terminals. The constants correspond to the attribute IDs defined in the ISOBUS Object Pool specification, enabling consistent and readable access to these identifiers in IEC 61499 applications.

## Interface Structure

Since `AID_OLBG` is a global constant definition rather than a function block, it does not contain event inputs, event outputs, data inputs/outputs, or adapters. Instead, it exposes a set of compile-time constants (all of type `USINT`) that can be referenced throughout an application. The following list enumerates these constants and their meanings.

### **Data Constants**

| Constant Name | Type | Value | Description |
|---------------|------|-------|-------------|
| `WIDTH` | `USINT` | `1` | Attribute ID for width in pixels (`AID_OLBG_WIDTH`). |
| `HEIGHT` | `USINT` | `2` | Attribute ID for height in pixels (`AID_OLBG_HEIGHT`). |
| `COLOUR` | `USINT` | `3` | Attribute ID for the colour index (`AID_OLBG_COLOUR`). |
| `TARGET_LINE_COLOUR` | `USINT` | `4` | Attribute ID for the target line colour (`AID_OLBG_TARGET_LINE_COLOUR`). |
| `OPTIONS` | `USINT` | `5` | Attribute ID for the options bitmask (`AID_OLBG_OPTIONS`). |
| `NUMB_TICKS` | `USINT` | `6` | Attribute ID for the number of ticks (`AID_OLBG_NUMB_TICKS`). |
| `MIN_VALUE` | `USINT` | `7` | Attribute ID for the minimum value (`AID_OLBG_MIN_VALUE`). |
| `MAX_VALUE` | `USINT` | `8` | Attribute ID for the maximum value (`AID_OLBG_MAX_VALUE`). |
| `VARIABLE_REF` | `USINT` | `9` | Attribute ID for the reference to a Variable object (`AID_OLBG_VARIABLE_REF`). |
| `TARGET_VAL_VAR_REF` | `USINT` | `10` | Attribute ID for the target value variable reference (`AID_OLBG_TARGET_VAL_VAR_REF`). |
| `TARGET_VALUE` | `USINT` | `11` | Attribute ID for the target value (`AID_OLBG_TARGET_VALUE`). |
| `VALUE` | `USINT` | `12` | Attribute ID for the current value (`AID_OLBG_VALUE`). |

## Functionality

The primary purpose of `AID_OLBG` is to provide a named, type-safe collection of ISOBUS attribute IDs for an output linear bar graph object. In ISOBUS object pool implementations, each visual object (such as a bar graph) has a set of configurable attributes, each identified by a numeric ID. The constants defined here map human-readable names to these numeric IDs, allowing application developers to:

- Refer to attribute IDs by meaningful names instead of raw numeric literals.
- Ensure consistency across multiple functions and blocks that interact with the same bar graph.
- Simplify maintenance by centralizing the attribute ID definitions in one location.

The underlying ISO 11783 specification defines these attributes as `AID_OLBG_*` IDs. This constant set is packaged under the `isobus::UT::Q::const::AID` namespace and is intended for use in ISOBUS virtual terminal implementations.

## Technical Features

- **Data Type Consistency**: All constants are declared as `USINT` (unsigned short integer), aligning with the ISOBUS attribute ID data type.
- **Namespace Organization**: The constants reside in the package `isobus::UT::Q::const::AID`, preventing name collisions and improving modularity.
- **Global Scope**: Declared as `VAR_GLOBAL CONSTANT`, the identifiers are accessible from any resource or function block within the IEC 61499 application.
- **Compile-Time Values**: As constants, their values are resolved at compile time, enabling compiler optimizations and reducing runtime overhead.
- **Documentation via Comments**: Each constant is annotated with its corresponding ISOBUS attribute ID name and description.

## State Overview

The `AID_OLBG` definition does not maintain runtime states or internal state machines. It is a static, constant collection that remains unchanged during execution. Its "state" is fixed at compile time and does not influence runtime behavior beyond providing constant values.

## Application Scenarios

Typical use cases for `AID_OLBG` include:

- **ISOBUS Virtual Terminal Displays**: Configuring a linear bar graph object on a virtual terminal by setting its attributes (size, colour, range, ticks, etc.).
- **Object Pool Construction**: Building or modifying an ISOBUS object pool where bar graph attribute IDs must be explicitly referenced.
- **Command/Response Handling**: Interpreting or constructing command messages that update bar graph properties using attribute IDs.
- **Cross-Component Communication**: Passing attribute IDs between function blocks that manage virtual terminal objects, ensuring consistent referencing.

For example, when a function block needs to update the `MIN_VALUE` attribute of a bar graph, it can use the constant `AID_OLBG.MIN_VALUE` instead of the literal `7`.

## Comparison with Similar Blocks

Within the same `isobus::UT::Q::const::AID` package, there may exist similar global constant definitions for other object types (e.g., output text, output meter, or input objects). In contrast to a function block that performs logic or data transformation, these constant definitions are pure data collections. Their main advantages include:

- **Clarity**: Named constants reduce the risk of using incorrect attribute IDs.
- **Maintainability**: Changes to attribute ID values (should the ISOBUS standard evolve) require a single edit.
- **Self-Documentation**: The declaration includes descriptive comments and initial values, making the mapping explicit.

Compared to using raw numeric literals within application logic, `AID_OLBG` ensures that all references are consistent and auditable.

## Conclusion

The `AID_OLBG` global constants definition provides a clean, standardized mechanism for referencing output linear bar graph attribute IDs in ISOBUS applications. By centralizing these IDs as named constants, it promotes code readability, maintainability, and correctness. It is an essential component for developers working with ISOBUS virtual terminals, serving as a reliable reference layer between the IEC 61499 application logic and the ISO 11783 object model.