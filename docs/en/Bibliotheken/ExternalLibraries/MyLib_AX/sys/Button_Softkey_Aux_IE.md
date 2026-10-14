# Button_Softkey_Aux_IE

![Button_Softkey_Aux_IE_network](./Button_Softkey_Aux_IE_network.svg)

* * * * * * * * * *

## Introduction

`Button_Softkey_Aux_IE` combines a non-latching VT button **or** a SoftKey **or** an AUX assignment (joystick) into one common `IND` event. The activation types are hard-wired (`BT_RELEASED_UNLATCHED`, `SK_RELEASED` and `AuxEnabled` respectively). The block corresponds to `Button_Aux_IE` and `Softkey_Aux_IE`, but serves all three sources at once. `Softkey_IE` and `Button_IE` are separate FORTE service interface blocks for different VT activation messages; a button never delivers an `SK_PRESSED`.

The button **must** be non-latching (`Latchable=0` in the pool); a latching button can sensibly be read only via `Button_IXA` (level).

![Button_Softkey_Aux_IE](Button_Softkey_Aux_IE.svg)

## Function Blocks (FBs) Used

### Sub-blocks: Button_Softkey_Aux_IE

- **Type**: SubAppType
- **Internal blocks used**:
    - **Button**: SubApp [`Button_IE_1`](./Button_IE_1.md) with `InputEvent=BT_RELEASED_UNLATCHED`
    - **Softkey**: SubApp [`Softkey_IE_1`](./Softkey_IE_1.md) with `InputEvent=SK_RELEASED`
    - **Aux**: SubApp [`Aux_IE_1`](./Aux_IE_1.md) with `InputEvent=AuxDisabled_START`
    - **MERGE**: `iec61499::events::E_MERGE_3` - merges the three `IND` events
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_3` - combines the `QO` outputs of `Button`, `Aux` and `Softkey` into the SubApp output `QO`
- **Imports**: `BT_RELEASED_UNLATCHED`, `AuxDisabled_START`, `SK_RELEASED`

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request; starts the chain `Button` → `Aux` → `Softkey`

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation; comes from `Softkey.INITO`
- **IND**: Fires when a button (`BT_RELEASED_UNLATCHED`), softkey (`SK_RELEASED`) or aux (`AuxEnabled`) is activated

### **Data Inputs**

- **QI** (`BOOL`): quality input, service enable (initial value `TRUE`); passed on to `Button.QI`, `Aux.QI` and `Softkey.QI`
- **u16ObjId** (`UINT`): Object ID of the softkey (`CSoftkey`, non-latching)
- **u16ObjIdA** (`UINT`): Object ID of the `AuxFunction2`
- **u16ObjIdB** (`UINT`): Object ID of the button (`CButton`, non-latching)

### **Data Outputs**

- **QO** (`BOOL`): quality output, `AND` of the `QO` outputs of `Button`, `Aux` and `Softkey` (via `AND_QO`)

## Program Flow and Connections

- `u16ObjId` → `Softkey.u16ObjId`; `Softkey.IND` → `MERGE.EI1`
- `u16ObjIdA` → `Aux.u16ObjId`; `Aux.IND` → `MERGE.EI2`
- `u16ObjIdB` → `Button.u16ObjId`; `Button.IND` → `MERGE.EI3`
- `MERGE.EO` → `IND`
- Initialization: `INIT` → `Button.INIT`; `Button.INITO` → `Aux.INIT`; `Aux.INITO` → `Softkey.INIT`; `Softkey.INITO` → `INITO` and → `AND_QO.REQ`
- Quality: `QI` → `Button.QI`, `Aux.QI` and `Softkey.QI`; `Button.QO` → `AND_QO.IN1`, `Aux.QO` → `AND_QO.IN2`, `Softkey.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`

## Technical Details

- Version 1.1 (2026-09-29): `packageName` typo fixed (`MyLib_AX::sys` → `MyLib::sys`), instance `Softkey_IE_1` renamed to `Softkey`, comment corrected (it described only 2 of 3 sources)
- Version 1.0 (2026-09-20): bugfix for `Automatik_Toggle_Button`, which previously used `Softkey_Aux_IE` with a button object ID

## Application Scenarios

- A control element that can be triggered via a button, a SoftKey or an AUX input

## Summary

`Button_Softkey_Aux_IE` bundles `Button_IE_1`, `Softkey_IE_1` and `Aux_IE_1` via an `E_MERGE_3` into a single `IND` event.
