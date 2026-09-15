# AID_IB

![AID_IB](./AID_IB.svg)

* * * * * * * * * *

## Introduction

The `AID_IB` type is a global constant definition that provides the standardized attribute identifiers for **Input Boolean objects** within the ISOBUS (ISO 11783) Universal Terminal (UT) protocol. These constants serve as semantic keys for accessing and querying the various attributes of an input boolean UI element. By centralizing these identifiers, the type ensures consistent and maintainable code when constructing or modifying ISOBUS object pools.

This definition is part of the `isobus::UT::Q::const::AID` package and adheres to the IEC 61499-1 standard for data representation.

## Interface Structure

Since `AID_IB` is a global constant type rather than a traditional function block, it does not expose event or data ports for execution. Instead, it provides a set of immutable constant values that are referenced globally by application logic and service interface function blocks when handling input boolean objects.

### **Event Inputs**

None. This type defines constants only and has no event-driven behaviour.

### **Event Outputs**

None. No event generation occurs from this type.

### **Data Inputs**

None in the conventional FB sense. The constants defined within this type serve as reference data values that are used as inputs to other function blocks or service sequences.

### **Data Outputs**

None in the conventional FB sense. The constant values are accessed directly by name from any part of the application where the type is referenced.

### **Adapters**

No adapters are provided. The constants are intended to be used as direct scalar references in ISOBUS-related processing blocks.

## Functionality

The `AID_IB` constant set enumerates the six standard attribute identifiers defined for an Input Boolean object in the ISO 11783-6 specification. In accordance with ISO 11783-6, attribute IDs enclosed in square brackets `[ ]` (such as `[5]` `VALUE` and `[6]` `ENABLED`) indicate **read-only attributes** for the *Change Attribute* command, accessible via the *Get Attribute Value* message (F.58):

| Constant Name | Value | Description |
|---|---|---|
| `BACKGROUND_COLOUR` | 1 | Index of the background colour used by the input boolean object. (Writable via *Change Attribute* F.38) |
| `WIDTH` | 2 | Width of the object in pixels. (Writable via *Change Attribute* F.38) |
| `FG_COLOUR` | 3 | Object ID of a Font Attributes object used to determine the font colour for the displayed text or symbol. (Writable via *Change Attribute* F.38) |
| `VARIABLE_REF` | 4 | Object ID of a Number Variable object to which the input value is bound. If NULL, the value is stored directly in the object. (Writable via *Change Attribute* F.38) |
| `VALUE` | [5] | **Read-only** for *Change Attribute* (ISO 11783-6). Value of the input field: `0` represents FALSE, any value greater than zero represents TRUE. Queryable via *Get Attribute Value* (F.58); updated at runtime using the specialized *Change Numeric Value* command (F.22). |
| `ENABLED` | [6] | **Read-only** for *Change Attribute* (ISO 11783-6). Operational state of the object: `0` = disabled, `1` = enabled. Queryable via *Get Attribute Value* (F.58); managed via *Select Input Object* (F.6) or state commands. |

Use of these constants avoids hard-coded magic numbers in application code, improves readability, and ensures that attribute references remain aligned with the ISOBUS specification.

## Technical Features

- **Data type:** All constants are declared as `USINT` (unsigned short integer, 8-bit range 0–255), sufficient for all defined attribute identifiers.
- **Immutability:** All constants are declared as `CONSTANT`, preventing accidental modification at runtime.
- **Standard compliance:** The identifiers correspond to the attribute IDs defined by the ISO 11783-6 / ISOBUS UT standard for the Input Boolean object type.
- **Namespace packaging:** The constants reside in the `isobus::UT::Q::const::AID` package, allowing clean import and preventing naming collisions.
- **Inline documentation:** Each constant carries a descriptive comment and its corresponding numeric attribute ID for quick cross-referencing.
- **Direct initialisation:** Initial values are assigned using type-qualified literals (e.g., `USINT#1`), ensuring type safety.

## State Overview

As a set of compile-time constants, `AID_IB` has no runtime state machine. The values are fixed at compile time and remain invariant for the lifetime of the application. The only relevant "state" is the numeric value of each attribute identifier, which must match the ISOBUS UT specification for correct behaviour.

## Application Scenarios

Typical use cases for the `AID_IB` constants include:

- **Object pool generation:** When building an ISOBUS Virtual Terminal object pool, these constants are used as attribute selectors in service requests (e.g., setting the background colour of an input boolean button).
- **Attribute querying via `GetAttribute` (F.58):** Applications that need to query the state or value of an input boolean object reference `AID_IB.VALUE` (`[5]`) or `AID_IB.ENABLED` (`[6]`) in *Get Attribute Value* request payloads.
- **Runtime value modification via `Change Numeric Value` (F.22):** To dynamically change the boolean input value at runtime, applications transmit a *Change Numeric Value* command (F.22) rather than a *Change Attribute* command, as `VALUE` (`[5]`) is designated read-only for *Change Attribute*.
- **Modifying appearance via `Change Attribute` (F.38):** Dynamic modifications to visual properties (such as `BACKGROUND_COLOUR`, `WIDTH`, `FG_COLOUR`, or `VARIABLE_REF`) reference the corresponding unbracketed AID constants in *Change Attribute* command payloads.
- **Value binding:** The `VARIABLE_REF` constant is used to link an input boolean object to a Number Variable object, enabling bidirectional data exchange between the terminal and the implement controller.

## Comparison with Similar Blocks

The ISOBUS UT specification defines attribute ID sets for several object types. `AID_IB` covers the Input Boolean object specifically. Comparable constant sets exist for:

- **Input Numeric (AID_IN):** Uses additional attributes such as decimal places, minimum/maximum value, and format for numeric input fields.
- **Output Boolean (AID_OB):** Shares attributes like `BACKGROUND_COLOUR`, `FG_COLOUR`, and `WIDTH` but adds attributes for flash state, line selection indication, and active state colour.
- **Object itself (AID_OBJ):** Represents generic object attributes common to all UT objects, such as position, size, and visibility.

Unlike those broader sets, `AID_IB` focuses exclusively on the boolean input semantics, and omits properties that are only relevant to output or numeric objects — keeping the constant list concise and purpose-specific.

## Conclusion

The `AID_IB` global constant type provides a clean, standardised, and type-safe way to reference Input Boolean object attribute identifiers in ISOBUS Universal Terminal applications. Its use eliminates magic numbers, improves code readability and maintainability, and guarantees alignment with the ISO 11783-6 protocol specifications.
