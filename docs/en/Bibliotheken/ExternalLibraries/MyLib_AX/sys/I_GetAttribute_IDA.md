# I_GetAttribute_IDA

![I_GetAttribute_IDA_network](./I_GetAttribute_IDA_network.svg)

* * * * * * * * * *

## Introduction

`I_GetAttribute_IDA` bundles `I_GetAttribute` (command *GetAttribute*, F.58) and `Attribute_IDA` (asynchronous adapter reception) with the shared parameters `u16ObjId` and `u8AID`. With `REQ`, the attribute of a VT object is requested; the response arrives asynchronously via the adapter `IN`.

![I_GetAttribute_IDA](I_GetAttribute_IDA.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request (to `Attribute_IDA.INIT`)
- **REQ**: Service request (to `GetAttribute.REQ`)

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation (from `Attribute_IDA.INITO`)

The response itself arrives via the adapter plug.

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, service enable (to `Attribute_IDA.QI`)
- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID
- **u8AID** (`USINT`): Attribute ID of the object

### **Data Outputs**

- **QO** (`BOOL`): quality output (from `Attribute_IDA.QO`)

### **Adapters**

- **IN** (plug, `adapter::types::unidirectional::AD`): Input data (response) from the resource

## Function Blocks (FBs) Used

- **GetAttribute**: `isobus::UT::I::I_GetAttribute` - sends the command *GetAttribute*
- **Attribute_IDA**: `isobus::UT::io::Attribute::Attribute_IDA` (`PARAMS=''`, `QI` comes from the SubApp interface) - receives the response asynchronously as an adapter

## Program Flow and Connections

- `REQ` → `GetAttribute.REQ`
- `u16ObjId` → `GetAttribute.u16ObjId` and `Attribute_IDA.u16ObjId`
- `u8AID` → `GetAttribute.u8AID` and `Attribute_IDA.u8AID`
- `Attribute_IDA.IN` → `IN`
- `INIT` → `Attribute_IDA.INIT`; `Attribute_IDA.INITO` → `INITO`
- `QI` → `Attribute_IDA.QI`; `Attribute_IDA.QO` → `QO`

## Technical Details

- The shared parameters ensure that request and reception refer to the same object and attribute.

## Application Scenarios

- Reading an attribute of a VT object (e.g. current value, color) and processing the response as an `AD` adapter value

## Summary

`I_GetAttribute_IDA` combines the request and reception of a VT object attribute in one SubApp.
