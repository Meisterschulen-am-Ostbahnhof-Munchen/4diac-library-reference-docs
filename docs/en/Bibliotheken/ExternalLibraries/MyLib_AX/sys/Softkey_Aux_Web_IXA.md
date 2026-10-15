# Softkey_Aux_Web_IXA

![Softkey_Aux_Web_IXA_network](./Softkey_Aux_Web_IXA_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Web_IXA` merges a VT softkey, an AUX assignment (joystick) and a local web override (e.g. `vt-ui-mirror` on the same module) into **one common, continuous AX level** - without a remote write and without an event. It is intended for jog operation, where the level itself (not an edge) is to be processed further, e.g. as the `G` input of an `AX_AX_SEL_AR` or as `DI1` of a GreenWhiteBackground feedback. It corresponds to the command half of `Softkey_Aux_IXA_TO_Remote_WRITE`, but provides `OUT` instead of a remote write.

![Softkey_Aux_Web_IXA](Softkey_Aux_Web_IXA.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation

Apart from `INITO` there are no event outputs - user events run via the adapter plug.

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE enables the service
- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID of the softkey (VT)
- **u16ObjIdA** (`UINT`, default `ID_NULL`): Object ID of the AuxFunction2 (VT)
- **ID** (`WSTRING`): Local subscribe address for a web client (e.g. `vt-ui-mirror`) on the same module, ORed with softkey and AUX

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - AND of `Softkey_IXA.QO`, `Aux_IXA.QO` and `SUBSCRIBE_WEB.QO`

### **Adapters**

- **OUT** (plug, `adapter::types::unidirectional::AX`): Softkey OR aux OR web override, continuous level (`TRUE` as long as held/active)

## Function Blocks (FBs) Used

- **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA`
- **Aux_IXA**: `isobus::UT::io::Auxiliary::IN::Aux_IXA`
- **SUBSCRIBE_WEB**: `adapter::net::AX_SUBSCRIBE_1`
- **OR_MERGE**: `adapter::booleanOperators::AX_OR_3`
- **AND_QO**: `iec61131::booleanOperators::AND_BOOL_3`

## Program Flow and Connections

- `u16ObjId` → `Softkey_IXA.u16ObjId`; `u16ObjIdA` → `Aux_IXA.u16ObjId`; `ID` → `SUBSCRIBE_WEB.ID`
- `Softkey_IXA.IN` → `OR_MERGE.IN1`
- `Aux_IXA.IN` → `OR_MERGE.IN2`
- `SUBSCRIBE_WEB.OUT` → `OR_MERGE.IN3`
- `OR_MERGE.OUT` → `OUT`
- `INIT` → `Softkey_IXA.INIT`; `Softkey_IXA.INITO` → `Aux_IXA.INIT`; `Aux_IXA.INITO` → `SUBSCRIBE_WEB.INIT`; `SUBSCRIBE_WEB.INITO` → `INITO` (and → `AND_QO.REQ`)
- `QI` → `Softkey_IXA.QI`, `Aux_IXA.QI` and `SUBSCRIBE_WEB.QI`
- `Softkey_IXA.QO` → `AND_QO.IN1`; `Aux_IXA.QO` → `AND_QO.IN2`; `SUBSCRIBE_WEB.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`

## Technical Details

- Continuous level instead of an edge: `OUT` stays `TRUE` as long as at least one source is active.
- No remote write and no user event output (only `INITO`).

## Application Scenarios

- Jog operation where the level is to go to a selector or a background color feedback

## Summary

`Softkey_Aux_Web_IXA` ORs softkey, aux and web override into one AX level.
