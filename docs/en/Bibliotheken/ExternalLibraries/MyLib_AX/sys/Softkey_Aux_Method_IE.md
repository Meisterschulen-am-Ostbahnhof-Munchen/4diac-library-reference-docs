# Softkey_Aux_Method_IE

![Softkey_Aux_Method_IE_network](./Softkey_Aux_Method_IE_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Method_IE` merges three triggers into **one common `IND` event**: a VT softkey, an AUX function (joystick, via `Softkey_Aux_IE`) or a **local argument-less OPC-UA method** (`ACTION=CREATE_METHOD`, `SERVER_0`) that is called e.g. by the web client (`vt-ui-mirror`) or by the OPC dashboard. The block builds on `Softkey_Aux_IE` and adds the method channel. Unlike `Softkey_Aux_Web_IE` (web via `SUBSCRIBE_R_TRIG` on a written variable), the web/dashboard trigger here is a genuine RPC method call without a shared variable.

![Softkey_Aux_Method_IE](Softkey_Aux_Method_IE.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation
- **IND**: Fires when the softkey, AUX or the local method is activated/called

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE enables the service
- **u16ObjId** (`UINT`): Object ID of the softkey
- **u16ObjIdA** (`UINT`): Object ID of the AuxFunction2
- **ID_METHOD** (`WSTRING`): Local method address (`ACTION=CREATE_METHOD`) for the argument-less trigger; called by the web client (`vt-ui-mirror`) and directly by the OPC dashboard

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - AND of `METHOD_SERVER.QO` and `SoftkeyAux.QO`

### **Adapters**

This function block uses no adapters.

## Function Blocks (FBs) Used

- **SoftkeyAux**: SubApp `MyLib::sys::Softkey_Aux_IE` - softkey or AUX
- **METHOD_SERVER**: `iec61499::net::SERVER_0` - local method
- **MERGE**: `iec61499::events::E_MERGE_2` - merges the two triggers
- **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` - combines `METHOD_SERVER.QO` and `SoftkeyAux.QO` into `QO`

## Program Flow and Connections

- `SoftkeyAux.IND` → `MERGE.EI1`
- `METHOD_SERVER.IND` → `MERGE.EI2`
- `METHOD_SERVER.IND` → `METHOD_SERVER.RSP` (feedback wiring)
- `MERGE.EO` → `IND`
- `u16ObjId`, `u16ObjIdA` → `SoftkeyAux`; `ID_METHOD` → `METHOD_SERVER.ID`
- `INIT` → `METHOD_SERVER.INIT`; `METHOD_SERVER.INITO` → `SoftkeyAux.INIT`; `SoftkeyAux.INITO` → `INITO` (and → `AND_QO.REQ`)
- `QI` → `METHOD_SERVER.QI` and `SoftkeyAux.QI`
- `METHOD_SERVER.QO` → `AND_QO.IN1`; `SoftkeyAux.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`

## Technical Details

- **`RSP` must be wired back to `IND`.** The trigger is argument-less and needs no processing; without this connection, FORTE's `onServerMethodCall` blocks the OPC-UA server thread for up to 4 s while holding open62541's global `serviceMutex`, which every `AX_PUBLISH_1` on another FORTE thread also needs (see `Ventilsteuerung/OPC_UA_VERNETZUNG.md`, item 12).

## Application Scenarios

- Key, joystick assignment and web/dashboard call that trigger the same action (e.g. "All off")

## Summary

`Softkey_Aux_Method_IE` combines softkey, AUX and an OPC-UA method call into one common `IND` event.
