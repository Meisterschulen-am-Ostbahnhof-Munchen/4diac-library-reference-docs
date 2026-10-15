# Softkey_Aux_Button_IE

![Softkey_Aux_Button_IE_network](./Softkey_Aux_Button_IE_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Button_IE` combines `Softkey_Aux_IE` (SoftKey OR AUX) and an additional button (`Button_IE`, e.g. the IO diagnostics test button, but it can also be a regular button) into **one** common `IND` event. It corresponds to `Softkey_Aux_Method_Button_IE` but without the local OPC-UA method and is intended for cases that only need SoftKey, AUX and button with equal rank.

![Softkey_Aux_Button_IE](Softkey_Aux_Button_IE.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirm
- **IND**: fires when the SoftKey, AUX **or** the button is activated

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, TRUE enables the service
- **u16ObjId** (UINT): object ID of the SoftKey
- **u16ObjIdA** (UINT): object ID of the AuxFunction2
- **u16ObjIdB** (UINT): object ID of a further button (e.g. IO diagnostics test button), equal in rank to SoftKey/AUX

### **Data Outputs**

- **QO** (`BOOL`): quality output, AND combination (`AND_QO`) of the `QO` outputs of `Button_IE_B` and `SoftkeyAux`

### **Adapters**

No adapters are used.

## Functionality

The network contains four blocks:

- `SoftkeyAux` (`MyLib::sys::Softkey_Aux_IE`): `u16ObjId` → `SoftkeyAux.u16ObjId`, `u16ObjIdA` → `SoftkeyAux.u16ObjIdA`; its `IND` goes to `MERGE.EI1`.
- `Button_IE_B` (`isobus::UT::io::Button::Button_IE`, `InputEvent = BT_RELEASED_UNLATCHED`): `u16ObjIdB` → `Button_IE_B.u16ObjId`; its `IND` goes to `MERGE.EI2`.
- `MERGE` (`iec61499::events::E_MERGE_2`): `MERGE.EO` → `IND`.
- `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`): combines `Button_IE_B.QO` (`IN1`) and `SoftkeyAux.QO` (`IN2`) into `QO`.

Initialization: `INIT` → `Button_IE_B.INIT`; `Button_IE_B.INITO` → `SoftkeyAux.INIT`; `SoftkeyAux.INITO` → `INITO` and → `AND_QO.REQ`. Quality: `QI` → `Button_IE_B.QI` and `SoftkeyAux.QI`; `AND_QO.OUT` → `QO`.

## Technical Details

- The additional button triggers on release (`BT_RELEASED_UNLATCHED`)
- Extracted from `Baender_Individual_Toggle.SUB`, where the same combination (`Softkey_Aux_IE` + `Button_IE` + `E_MERGE_2`) was wired three times for hedgehog belt, elevator and cross belt
- Stateless

## Application Scenarios

- An output that is to be triggered with equal rank by SoftKey, joystick assignment and an additional button (e.g. IO diagnostics test button)

## Conclusion

`Softkey_Aux_Button_IE` merges SoftKey, AUX and a further button into a single `IND` event.
