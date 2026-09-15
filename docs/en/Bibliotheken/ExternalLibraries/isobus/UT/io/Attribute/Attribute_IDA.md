# Attribute_IDA

* * * * * * * * * *

## Introduction

The **Attribute_IDA** is a composite adapter wrapper function block around `Attribute_ID`. It provides received object attribute data via a unidirectional `AD` adapter plug (`IN`) for adapter-native networks.

![Attribute_IDA](Attribute_IDA.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Service initialization (associated with `QI`, `PARAMS`, `u16ObjId`, `u8AID`)
- `REQ`: Service request (associated with `QI`)

### **Event Outputs**

- `INITO`: Initialization confirm (associated with `QO`, `STATUS`)

### **Data Inputs**

- `QI` (BOOL): Event input qualifier
- `PARAMS` (STRING): Service parameters
- `u16ObjId` (UINT): Object ID
- `u8AID` (USINT): Attribute ID

### **Data Outputs**

- `QO` (BOOL): Event output qualifier
- `STATUS` (STRING): Operational status message

### **Adapters**

- `IN` (AD Plug): Unidirectional DWORD adapter plug providing attribute data

## Functionality

`Attribute_IDA` encapsulates `Attribute_ID` internally. Upon incoming `IND` or `CNF` events, it automatically routes the events and 32-bit attribute data to the `AD` adapter interface (`IN.E1` and `IN.D1`).

## Technical Features

✔ **Adapter-native interface** (`AD` plug)
✔ **Asynchronous event routing** (`IND`/`CNF` -> `IN.E1`)
✔ **Seamless integration into adapter networks**
