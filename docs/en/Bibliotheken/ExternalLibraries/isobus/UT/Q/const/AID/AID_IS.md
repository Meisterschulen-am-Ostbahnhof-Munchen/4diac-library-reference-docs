# AID_IS

![AID_IS](./AID_IS.svg)

* * * * * * * * * *

## Introduction

The `AID_IS` global constant set defines attribute identifiers used for **Input String objects** in the context of ISOBUS (ISO 11783-6) Universal Terminal (UT) applications. These constants allow standardized access to the properties of an input string object, such as width, height, background colour, font attributes, and justification. They are defined as compile-time constants of type `USINT`.

## Interface Structure

This element is a global constants set and does not conform to the traditional function block interface (no events, no data I/O, no adapters).

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

The following constants are provided. In accordance with ISO 11783-6 Table B.17, attribute IDs enclosed in square brackets `[ ]` (such as `[9]` `ENABLED`) indicate **read-only attributes** for the *Change Attribute* command, accessible via the *Get Attribute Value* message (F.58):

| Name | Type | Value | Description |
|---|---|---|---|
| `WIDTH` | USINT | 1 | Width in pixels. (Writable via *Change Attribute* F.38) |
| `HEIGHT` | USINT | 2 | Height in pixels. (Writable via *Change Attribute* F.38) |
| `BACKGROUND_COLOUR` | USINT | 3 | Background colour index. (Writable via *Change Attribute* F.38) |
| `FONT_ATT` | USINT | 4 | Object ID of a Font Attributes object. (Writable via *Change Attribute* F.38) |
| `INP_ATT` | USINT | 5 | Object ID of an Input Attributes / Extended Input Attributes object. (Writable via *Change Attribute* F.38) |
| `OPTIONS` | USINT | 6 | Options bitmask. (Writable via *Change Attribute* F.38) |
| `VARIABLE_REF` | USINT | 7 | Object ID of a String Variable object. (Writable via *Change Attribute* F.38) |
| `JUSTIFICATION` | USINT | 8 | Justification: Bits 0-1 (Horizontal: 0=Left, 1=Middle, 2=Right), Bits 2-3 (Vertical: 0=Top, 1=Middle, 2=Bottom). (Writable via *Change Attribute* F.38) |
| `ENABLED` | USINT | [9] | **Read-only** for *Change Attribute* (ISO 11783-6). 0 = Disabled, 1 = Enabled. Queryable via *Get Attribute Value* (F.58); managed via *Select Input Object* (F.6) or state commands. |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The `AID_IS` constants act as predefined attribute identifiers for an input string object within an ISOBUS UT object pool. By using these constants, application logic can unambiguously refer to specific properties when reading or modifying the object’s attributes via standard ISOBUS service interfaces.

## Technical Features

- **Type:** All constants are of type `USINT` (unsigned 8-bit integer).
- **Standard compliance:** Identifiers match the ISO 11783-6 specification for input string object attributes.
- **Enclosed square brackets `[ ]`:** Mark attributes (such as `[9]` `ENABLED`) that are read-only for *Change Attribute*.
- **Compile-time constants:** Resolved during compilation, offering performance benefits and guaranteeing immutability.

## State Overview

This global constant set does not maintain any state. It provides static values that are available globally throughout the application.

## Application Scenarios

The `AID_IS` constants are used in ISOBUS Universal Terminal applications to:

- Configure an input string object’s dimensions or visual appearance via *Change Attribute* (F.38).
- Change font attributes or background colour.
- Set or retrieve the associated string variable reference.
- Query the operational enabled state (`AID_IS.ENABLED` `[9]`) via *Get Attribute Value* (F.58) or manage focus via *Select Input Object* (F.6).

## Conclusion

The `AID_IS` global constant set provides a clear and standardized way to reference attributes of input string objects in ISOBUS UT applications.
