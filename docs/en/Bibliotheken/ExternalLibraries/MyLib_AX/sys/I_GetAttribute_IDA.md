# I_GetAttribute_IDA

![I_GetAttribute_IDA_network](./I_GetAttribute_IDA_network.svg)

* * * * * * * * * *

## Introduction

`I_GetAttribute_IDA` bundles `I_GetAttribute` (command *GetAttribute*, F.58) and `Attribute_IDA` (asynchronous adapter reception) with the shared parameters `u16ObjId` and `u8AID`. With `REQ`, the attribute of a VT object is requested; the response arrives asynchronously via the adapter `IN`.

![I_GetAttribute_IDA](I_GetAttribute_IDA.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Service request (to `GetAttribute.REQ`)

### **Event Outputs**

*No direct event outputs - the response arrives via the adapter plug*

### **Data Inputs**

- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID
- **u8AID** (`USINT`): Attribute ID of the object

### **Data Outputs**

*No data outputs*

### **Adapters**

- **IN** (plug, `adapter::types::unidirectional::AD`): Input data (response) from the resource

## Function Blocks (FBs) Used

- **GetAttribute**: `isobus::UT::I::I_GetAttribute` - sends the command *GetAttribute*
- **Attribute_IDA**: `isobus::UT::io::Attribute::Attribute_IDA` (`QI=TRUE`, `PARAMS=''`) - receives the response asynchronously as an adapter

## Program Flow and Connections

- `REQ` → `GetAttribute.REQ`
- `u16ObjId` → `GetAttribute.u16ObjId` and `Attribute_IDA.u16ObjId`
- `u8AID` → `GetAttribute.u8AID` and `Attribute_IDA.u8AID`
- `Attribute_IDA.IN` → `IN`

## Technical Details

- The shared parameters ensure that request and reception refer to the same object and attribute.

## Application Scenarios

- Reading an attribute of a VT object (e.g. current value, color) and processing the response as an `AD` adapter value

## Summary

`I_GetAttribute_IDA` combines the request and reception of a VT object attribute in one SubApp.
