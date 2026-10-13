# Attribute_ID

* * * * * * * * * *

## Introduction

`Attribute_ID` is an input service interface function block for double word input data (`DWORD`). It reads an attribute of a VT object (object ID `u16ObjId`, attribute ID `u8AID`) and provides its value as `DWORD` at output `IN`.

![Attribute_ID](Attribute_ID.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): service initialisation
  - Associated with: `QI`, `PARAMS`, `u16ObjId`, `u8AID`
- **REQ**: service request
  - Associated with: `QI`

### **Event Outputs**

- **INITO** (`EInit`): initialisation confirmation
  - Associated with: `QO`, `STATUS`
- **CNF**: confirmation of the requested service
  - Associated with: `QO`, `STATUS`, `IN`
- **IND**: indication from the resource
  - Associated with: `QO`, `STATUS`, `IN`

### **Data Inputs**

- **QI** (`BOOL`): event input qualifier
- **PARAMS** (`STRING`): service parameters
- **u16ObjId** (`UINT`): object ID, initial value `ID_NULL`
- **u8AID** (`USINT`): attribute ID of the object

### **Data Outputs**

- **QO** (`BOOL`): event output qualifier
- **STATUS** (`STRING`): service status
- **IN** (`DWORD`): input data from the resource

### **Adapters**

No adapter interfaces available.

## Functionality

The function block is a service interface FB without an internal implementation in the type (interface declaration). `INIT` initialises the service with `QI`, `PARAMS`, `u16ObjId` and `u8AID` and is confirmed with `INITO`. `REQ` issues a request that is confirmed with `CNF`; `IND` signals an indication from the resource. In all three cases `QO`, `STATUS` and (for `CNF`/`IND`) the value `IN` are output.

## Technical Details

- Uses the data type `DWORD` (32 bit) for the attribute value
- The attribute ID to read is given via the data input `u8AID`
- The object ID is preset with `ID_NULL` (`isobus::UT::Q::const::IDs::ID_NULL`)
- Originally created by fortiss GmbH (Monika Wenger, 2017) as an input service interface

## Application Scenarios

- Reading attribute values (e.g. attribute ID of an object in the VT) in ISOBUS applications

## ⚖️ Comparison with Similar Building Blocks

- [NumericValue_ID](../NumericValue/NumericValue_ID.md) has the same service interface but reads the numeric value of an object and has no `u8AID` input.

## Conclusion

`Attribute_ID` delivers the attribute value of a VT object as `DWORD` through a service interface.
