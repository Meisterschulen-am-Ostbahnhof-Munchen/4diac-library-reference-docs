# I_GetAttribute_IDA

## Introduction

The `I_GetAttribute_IDA` SubApp encapsulates the ISO 11783-6 GetAttribute command (`I_GetAttribute`, F.58) and the asynchronous adapter receiver (`Attribute_IDA`) into a reusable SubApp type (`MyLib::sys`). It exposes `u16ObjId` and `u8AID` once as parameters and delivers received attribute data via the `IN` adapter plug.

## Interface Structure

### **SubAppEventInputs**

- `REQ`: Attribute query request (transmits command F.58 to the VT)

### **InputVars**

- `u16ObjId` (UINT): Object ID of the target VT object (e.g. `InputNumber_I1`)
- `u8AID` (USINT): Attribute ID (e.g. `AID_IN.VALUE`)

### **Plugs**

- `IN` (`adapter::types::unidirectional::AD`): Asynchronously received attribute data adapter plug

## Functionality

1. Upon arrival of `REQ`, the internal `GetAttribute` block sends the GetAttribute command (F.58) to the VT.
2. When the VT asynchronously responds with the attribute value, `Attribute_IDA` captures the response and forwards it via the `IN` adapter plug (e.g., to `AD_TO_AR_NUM`).
