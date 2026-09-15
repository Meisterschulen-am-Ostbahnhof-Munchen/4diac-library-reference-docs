# I_GetAttribute

![I_GetAttribute](https://user-images.githubusercontent.com/116869307/214147879-2749e8c2-364e-4335-9c0e-0445694831e4.png)

* * * * * * * * * *

## Introduction

The **I_GetAttribute** is a standards-compliant function module for querying object attributes in virtual terminals, developed under the EPL-2.0 license. Version 1.0 implements the ISO 11783-6 (Part 6 - F.58) specification for VT systems from version 4 onwards.
![I_GetAttribute](I_GetAttribute.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Initialization request (with object ID)
- `REQ`: Attribute query request (with attribute ID)

### **Event Outputs**

- `INITO`: Initialization acknowledgment
- `CNF`: Query acknowledgment (with `STATUS` and `s16result`)

### **Data Inputs**

- `u16ObjId` (UINT): Object ID (16-bit)
- `u8AID` (USINT): Attribute ID (8-bit)

### **Data Outputs**

- `STATUS` (STRING): Operational status message
- `s16result` (INT): ISO-compliant result code (0 = OK, negative values = error)

## Valid Object IDs & Validation

The Get Attribute Value command (F.58) is a fixed 8-byte message (Object ID in bytes 2,3, Attribute ID in Byte 4; no Transport Protocol). `INIT` validates both the object type (`iso_has_readable_attribute_id` – does this object type have any readable AID at all, read-only or writable?) and the specific Attribute ID against that type's attribute table (`iso_is_readable_attribute`), before the command is ever sent — unlike *Change Attribute*, both read-only and writable AIDs are accepted here.

The underlying C implementation (`cmd_get_attribute_value`) performs no type check of its own (only `ID_NULL` handling). The VT additionally validates the Object ID and the Attribute ID on its side and returns an error response if either is invalid.

`ID_NULL` (65535) is not a command target but deactivates the FB when sent via `INIT` (`VT_E_DEACTIVATED`).

## Functionality

1. **Initialization**:
   - `INIT` with object ID (`u16ObjId`)
   - `INITO` confirms operational readiness

2. **Attribute Query (Asynchronous Event)**:
   - `REQ` triggers the query for the specified attribute ID (`u8AID`).
   - Since object attribute queries on the ISOBUS VT are **asynchronous events**, the response from the VT resource arrives asynchronously via an indication event (`IND` / `Attribute_ID`) or `CNF`.

3. **Error Handling**:
   - ISO-standardized error codes in `s16result`
   - Detailed status messages via `STATUS`

## Technical Features

✔ **ISO 11783-6 compliant** (F.58)
✔ **Asynchronous Event Handling** (Response indication via `IND` / `Attribute_ID`)
✔ **Exclusive to VT Version 4+**
✔ **Universally applicable** (All object types with readable AIDs)
✔ **Real-time capable** (Fast query cycles)

## Attribute Types

| Category         | Example IDs | Description                   |
| ---------------- | ----------- | ----------------------------- |
| Basic Attributes | 0x01 - 0x0F | Visibility, Activity          |
| Appearance       | 0x10 - 0x2F | Colors, Borders, Alignment    |
| Content          | 0x30 - 0x4F | Text Values, Numeric Values   |
| States           | 0x50 - 0x6F | Alarm Status, Operating Modes |

## Return Codes (s16result)

| Code | Constant                  | Meaning                |
| ---- | ------------------------- | ---------------------- |
| 0    | VT_E_NO_ERR               | Query successful       |
| -40  | VT_E_DEACTIVATED          | FB deactivated via ID_NULL on INIT |
| -132 | VT_E_INVALID_OBJECT_ID    | Object ID's type has no readable AID at all (`iso_has_readable_attribute_id`) |
| -133 | VT_E_INVALID_ATTRIBUTE_ID | Attribute ID is not a valid/readable AID for this object type (`iso_is_readable_attribute`) |
| -131 | VT_E_NOT_READY            | Buffered: INIT not yet completed / VT not yet ready |
| -6   | VT_E_OVERFLOW             | Buffer overflow        |
| -8   | VT_E_NOACT                | Command not possible in current state |
| -21  | VT_E_NO_INSTANCE          | No VT client available |
| -128 | VT_E_HANDLE_INVALID       | Error cause: Invalid handle |
| -129 | VT_E_ISO_INSTANCE_INVALID | Invalid VT instance    |
| -130 | VT_E_NOT_ALIVE            | VT instance valid, but VT dead |

## Application Scenarios

- **System Diagnostics**: Status Queries
- **User Interaction**: Input Value Validation
- **Automation**: Rule-Based Controls
- **Configuration**: Parameter Readout

## ⚖️ Comparison with Similar Function Blocks

| Feature         | I_GetAttribute | VtReadValue | VtObjectQuery |
| --------------- | -------------- | ----------- | ------------- |
| ISO Standard    | ✔              | ✖           | ✖             |
| VT Version      | 4+             | All         | All           |
| Attribute Width | Universal      | Value-Only  | Limited IDs   |

## Conclusion

The I_GetAttribute function block offers the standard implementation for attribute queries:

- **Efficient**: Minimal latency
- **Reliable**: Robust error detection
- **Flexible**: Supports all object types

Essential for:

- Diagnostic systems
- Automation solutions
- Interactive VT applications
- Configuration management
