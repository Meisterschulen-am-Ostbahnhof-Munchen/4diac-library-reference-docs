# Attribute_ID

* * * * * * * * * *

## Introduction

The **Attribute_ID** is an input service interface function block for VT object attribute data (UDINT). It serves as an input interface for receiving asynchronous indications (`IND`) and confirmations (`CNF`) of object attribute values in ISOBUS VT systems.

![Attribute_ID](Attribute_ID.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Service initialization (associated with `QI`, `PARAMS`, `u16ObjId`, `u8AID`)
- `REQ`: Service request (associated with `QI`)

### **Event Outputs**

- `INITO`: Initialization confirm (associated with `QO`, `STATUS`)
- `CNF`: Confirmation of requested service (associated with `QO`, `STATUS`, `u32ValueAttribute`, `s16result`)
- `IND`: Indication from resource (associated with `QO`, `STATUS`, `u32ValueAttribute`, `s16result`)

### **Data Inputs**

- `QI` (BOOL): Event input qualifier
- `PARAMS` (STRING): Service parameters
- `u16ObjId` (UINT): Object ID (16-bit)
- `u8AID` (USINT): Attribute ID (8-bit)

### **Data Outputs**

- `QO` (BOOL): Event output qualifier
- `STATUS` (STRING): Operational status message
- `u32ValueAttribute` (UDINT): Received attribute value (32-bit)
- `s16result` (INT): ISO-compliant result code

## Functionality

The block initializes via the `INIT` event. After initialization, it receives asynchronous attribute value indications (`IND`) from the VT resource as well as confirmations (`CNF`) for submitted requests. The attribute value is output on `u32ValueAttribute` as a 32-bit UDINT.

## Technical Features

✔ **ISO 11783-6 compliant**
✔ **Asynchronous event handling** (Reception via `IND` / `CNF`)
✔ **Universally applicable** (For all VT object and attribute IDs)
