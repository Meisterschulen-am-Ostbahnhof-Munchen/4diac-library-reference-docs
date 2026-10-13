# Softkey_Aux_Web_IXA

![Softkey_Aux_Web_IXA_network](./Softkey_Aux_Web_IXA_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Web_IXA` merges a VT softkey, an AUX assignment (joystick) and a local web override (e.g. `vt-ui-mirror` on the same module) into **one common, continuous AX level** - without a remote write and without an event. It is intended for jog operation, where the level itself (not an edge) is to be processed further, e.g. as the `G` input of an `AX_AX_SEL_AR` or as `DI1` of a GreenWhiteBackground feedback. It corresponds to the command half of `Softkey_Aux_IXA_TO_Remote_WRITE`, but provides `OUT` instead of a remote write.

![Softkey_Aux_Web_IXA](Softkey_Aux_Web_IXA.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs - events run via the adapter plug*

### **Data Inputs**

- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID of the softkey (VT)
- **u16ObjIdA** (`UINT`, default `ID_NULL`): Object ID of the AuxFunction2 (VT)
- **ID** (`WSTRING`): Local subscribe address for a web client (e.g. `vt-ui-mirror`) on the same module, ORed with softkey and AUX

### **Data Outputs**

*No data outputs*

### **Adapters**

- **OUT** (plug, `adapter::types::unidirectional::AX`): Softkey OR aux OR web override, continuous level (`TRUE` as long as held/active)

## Function Blocks (FBs) Used

- **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA` (`QI=TRUE`)
- **Aux_IXA**: `isobus::UT::io::Auxiliary::IN::Aux_IXA` (`QI=TRUE`)
- **SUBSCRIBE_WEB**: `adapter::net::AX_SUBSCRIBE_1` (`QI=TRUE`)
- **OR_MERGE**: `adapter::booleanOperators::AX_OR_3`

## Program Flow and Connections

- `u16ObjId` → `Softkey_IXA.u16ObjId`; `u16ObjIdA` → `Aux_IXA.u16ObjId`; `ID` → `SUBSCRIBE_WEB.ID`
- `Softkey_IXA.IN` → `OR_MERGE.IN1`
- `Aux_IXA.IN` → `OR_MERGE.IN2`
- `SUBSCRIBE_WEB.OUT` → `OR_MERGE.IN3`
- `OR_MERGE.OUT` → `OUT`

## Technical Details

- Continuous level instead of an edge: `OUT` stays `TRUE` as long as at least one source is active.
- No remote write and no event output.

## Application Scenarios

- Jog operation where the level is to go to a selector or a background color feedback

## Summary

`Softkey_Aux_Web_IXA` ORs softkey, aux and web override into one AX level.
