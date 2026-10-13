# Softkey_Aux_Button_IE

![Softkey_Aux_Button_IE_network](./Softkey_Aux_Button_IE_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Button_IE` combines `Softkey_Aux_IE` (SoftKey OR AUX) and an additional button (`Button_IE`, e.g. the IO diagnostics test button, but it can also be a regular button) into **one** common `IND` event. It corresponds to `Softkey_Aux_Method_Button_IE` but without the local OPC-UA method and is intended for cases that only need SoftKey, AUX and button with equal rank.

![Softkey_Aux_Button_IE](Softkey_Aux_Button_IE.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs.

### **Event Outputs**

- **IND**: fires when the SoftKey, AUX **or** the button is activated

### **Data Inputs**

- **u16ObjId** (UINT): object ID of the SoftKey
- **u16ObjIdA** (UINT): object ID of the AuxFunction2
- **u16ObjIdB** (UINT): object ID of a further button (e.g. IO diagnostics test button), equal in rank to SoftKey/AUX

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

No adapters are used.

## Functionality

The network contains three blocks:

- `SoftkeyAux` (`MyLib::sys::Softkey_Aux_IE`): `u16ObjId` → `SoftkeyAux.u16ObjId`, `u16ObjIdA` → `SoftkeyAux.u16ObjIdA`; its `IND` goes to `MERGE.EI1`.
- `Button_IE_B` (`isobus::UT::io::Button::Button_IE`, `QI = TRUE`, `InputEvent = BT_RELEASED_UNLATCHED`): `u16ObjIdB` → `Button_IE_B.u16ObjId`; its `IND` goes to `MERGE.EI2`.
- `MERGE` (`iec61499::events::E_MERGE_2`): `MERGE.EO` → `IND`.

## Technical Details

- The additional button triggers on release (`BT_RELEASED_UNLATCHED`)
- Extracted from `Baender_Individual_Toggle.SUB`, where the same combination (`Softkey_Aux_IE` + `Button_IE` + `E_MERGE_2`) was wired three times for hedgehog belt, elevator and cross belt
- Stateless

## Application Scenarios

- An output that is to be triggered with equal rank by SoftKey, joystick assignment and an additional button (e.g. IO diagnostics test button)

## Conclusion

`Softkey_Aux_Button_IE` merges SoftKey, AUX and a further button into a single `IND` event.
