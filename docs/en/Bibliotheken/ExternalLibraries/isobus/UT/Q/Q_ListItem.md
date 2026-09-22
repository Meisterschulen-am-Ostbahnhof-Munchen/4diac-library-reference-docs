# Q_ListItem

![Q_ListItem](https://user-images.githubusercontent.com/116869307/214147979-74d9863c-caa6-4953-8208-ae779dc6c71a.png)

* * * * * * * * * *

## Introduction

The **Q_ListItem** is a standards-compliant function module for managing list entries in virtual terminals, developed under the EPL-2.0 license. Version 1.0 implements the ISO 11783-6 (Part 6 - F.42) specification for list input objects in VT systems.

![Q_ListItem](Q_ListItem.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Initialization request (with list object ID `u16ObjId` and list index `u8ListIndex`)
- `REQ`: Change request (with new object ID `u16NewObjId`)

### **Event Outputs**

- `INITO`: Initialization confirmation
- `CNF`: Change confirmation (with status and previous object ID)

### **Data Inputs**

- `u16ObjId` (UINT): List object ID (16-bit)
- `u8ListIndex` (USINT): List index (0-based, provided at `INIT`)
- `u16NewObjId` (UINT): New object ID or 0xFFFF (empty entry)

### **Data Outputs**

- `STATUS` (STRING): Operational status message
- `u16OldObjId` (UINT): Previous object ID
- `s16result` (INT): ISO-compliant result code

## Instance Uniqueness

This block requires instance uniqueness regarding the pair (**u16ObjId**, **u8ListIndex**). Only one instance of `Q_ListItem` may exist in the entire program for the same list item entry. `u8ListIndex` is read at `INIT` — a second instance with the same list object ID and list index is deactivated during `INIT` (STATUS = "This objID is already in use"). See also [Instance Uniqueness](./INSTANCE_UNIQUENESS.md).

## Valid Object IDs

`u16ObjId` (the list object) and `u16NewObjId` (the new list item) are subject to **different** rules.

**`u16ObjId` — valid list object types (Annex F.42):**
Input List Field (10000–10999), Output List Object (37000–37999, VT v4+), Animation Object (44000–44999, VT v5+), External Object Definition (41000–41999, VT v5+).

**`u16NewObjId` — new list item:** any object ID (the item points to that object) or 0xFFFF (ID_NULL) to set an empty item. The validity of the new object ID is verified by the VT and reported back via F.43 (bit 2 = Invalid New List Item Object ID).

ID_NULL (65535) is not a valid command target for `u16ObjId` but deactivates the FB when used with `INIT`.

## Functionality

1. **Initialization**:
   - `INIT` with list object ID and list index
   - `INITO` confirms operational readiness

2. **List Change**:
   - `REQ` with new object ID
   - Updates the list item
   - `CNF` provides result status and previous object ID

3. **Special Case**:
   - `u16NewObjId = 0xFFFF` creates an empty entry

## Technical Features

✔ **ISO 11783-6 compliant** (F.42)
✔ **Dynamic list management** (real-time changes)
✔ **Pairwise uniqueness at INIT** (`u16ObjId` + `u8ListIndex`)
✔ **Flexible empty items** (0xFFFF special value)

## Index Reference

| Index | Description              |
| ----- | ------------------------ |
| 0     | First list item entry    |
| ...   |                          |
| 255   | Maximum index (uint8_t)  |

## Return Codes (s16result)

| Code | Constant                  | Meaning                  |
| ---- | ------------------------- | ------------------------ |
| 0    | VT_E_NO_ERR               | Successful change        |
| -6   | VT_E_OVERFLOW             | Buffer overflow          |
| -8   | VT_E_NOACT                | VT not ready             |
| -21  | VT_E_NO_INSTANCE          | No VT client available   |
| -129 | VT_E_ISO_INSTANCE_INVALID | Invalid VT instance      |
| -130 | VT_E_NOT_ALIVE            | VT not active            |

## Application Scenarios

- **Dynamic Menus**: Context-dependent items
- **Device Selection**: Plug-and-play device lists
- **Parameter Settings**: Selection options
- **Diagnostic Lists**: Error code management

## Conclusion

The Q_ListItem block provides efficient list management:

- **Reactive**: Instant GUI updates
- **Robust**: Integrated error handling
- **Compatible**: Full ISO compliance
