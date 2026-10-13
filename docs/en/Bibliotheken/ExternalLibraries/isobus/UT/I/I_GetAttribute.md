# I_GetAttribute

![I_GetAttribute](I_GetAttribute.svg)

* * * * * * * * * *

## Introduction

`I_GetAttribute` implements the **Get Attribute Value** command (ISO 11783-6, F.58). It is available from VT version 4 and is used by a working set to query the VT for the current state of objects within the VT.

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Service initialization
    - Associated with: `u16ObjId`, `u8AID`
- **REQ**: Service request

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation
- **CNF**: Confirmation of the requested service
    - Associated with: `STATUS`, `s16result`

### **Data Inputs**

- **u16ObjId** (`UINT`, initial value `ID_NULL`): Object ID
- **u8AID** (`USINT`): Attribute ID of the object

### **Data Outputs**

- **STATUS** (`STRING`): Service status
- **s16result** (`INT`): Return value (see below)

### **Adapters**

No adapter interfaces.

## How it works

The Get Attribute Value command (F.58) is a fixed 8-byte message (object ID in bytes 2 and 3, attribute ID in byte 4; no Transport Protocol).

Before the command is ever sent, `INIT` validates both the object type (`iso_has_readable_attribute_id`: does this object type have any attribute ID at all, read-only or writable?) and the specific attribute ID against that type's attribute table (`iso_is_readable_attribute`). Unlike Change Attribute, both read-only and writable attribute IDs are accepted here. The underlying C implementation (`cmd_get_attribute_value`) performs no type check of its own, only `ID_NULL` handling; the VT additionally validates the object ID and the attribute ID on its side and returns an error response if either is invalid.

`ID_NULL` (65535) is not a command target but can be used in `INIT` to deactivate the function block.

### Return values (`s16result`)

| Value | Meaning |
| :--- | :--- |
| `VT_E_NO_ERR` (0) | OK |
| `VT_E_DEACTIVATED` (-40) | FB deactivated via `ID_NULL` on `INIT` |
| `VT_E_INVALID_OBJECT_ID` (-132) | The object ID's type has no readable AID at all (`iso_has_readable_attribute_id`) |
| `VT_E_INVALID_ATTRIBUTE_ID` (-133) | Attribute ID is not a valid/readable AID for this object type (`iso_is_readable_attribute`) |
| `VT_E_NOT_READY` (-131) | Buffered: `INIT` not yet completed / VT not yet ready |
| `VT_E_OVERFLOW` (-6) | Buffer overflow |
| `VT_E_NOACT` (-8) | Command not possible in current state |
| `VT_E_NO_INSTANCE` (-21) | No VT client available |
| `VT_E_HANDLE_INVALID` (-128) | Error cause: invalid handle |
| `VT_E_ISO_INSTANCE_INVALID` (-129) | Identifier for instance (connection) invalid |
| `VT_E_NOT_ALIVE` (-130) | Identifier for instance valid, but VT dead |

## Technical Details

- **Instance uniqueness**: This block requires uniqueness with respect to the pair (`u16ObjId`, `u8AID`). Only one instance of `I_GetAttribute` may exist in the whole program for the same object/attribute pair. `u8AID` is read in at `INIT`; a second instance for the same object ID and attribute ID is deactivated at `INIT`.
- The block delivers only status and return value via `CNF`; the read attribute value is delivered by service interface blocks such as [`Attribute_IDA`](../io/Attribute/Attribute_IDA.md).

## Application Scenarios

- Querying the current state of an object in the VT (from VT version 4)

## Summary

`I_GetAttribute` sends the Get Attribute Value command (F.58) for an object/attribute pair and reports status and return value.
