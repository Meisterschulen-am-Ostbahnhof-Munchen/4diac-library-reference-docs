# AX_TO_QXA_BG_PUBLISH

![AX_TO_QXA_BG_PUBLISH_network](./AX_TO_QXA_BG_PUBLISH_network.svg)

* * * * * * * * * *

## Introduction

`AX_TO_QXA_BG_PUBLISH` drives, from an already finished AX state (e.g. merged by OR of several sources such as a staggered group signal, single toggle or IO diagnostics test button), **one** physical output, the background colour (SoftKey and AUX) and a local state publish for web client and OPC dashboard. It is the counterpart to `MyLib::sys::AX_SoftkeyToggle_RS_TO_QXA_BG`, which still generates the toggle/reset itself; here the AX state is already available.

![AX_TO_QXA_BG_PUBLISH](AX_TO_QXA_BG_PUBLISH.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

- **u16ObjId** (UINT): object ID of the SoftKey, carries the background colour (default `ID_NULL`)
- **u16ObjIdA** (UINT): object ID of the AuxFunction2 (joystick), also shows the background colour at the aux handle (default `ID_NULL`)
- **Output** (`logiBUS::io::DQ::logiBUS_DO_S`): selection of the output `Output_Q1`..`Output_Q12` (default `logiBUS_DO::Invalid`)
- **ID_STATE_WRITE** (WSTRING): local publish address (`ACTION=WRITE`) for the actual state; subscribed remotely by web client/dashboard

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **IN** (socket, `adapter::types::unidirectional::AX`): finished, already merged AX state; distributed to output, background and publish

## Functionality

`IN` is routed to an `AX_SPLIT_3` (`adapter::events::unidirectional::AX_SPLIT_3`), which distributes the state to three destinations:

- `AX_SPLIT_3.OUT1` → `DigitalOutput.OUT`: `DigitalOutput` (`logiBUS::io::DQ::logiBUS_QXA`, `QI = TRUE`) switches the physical output; `Output` → `DigitalOutput.Output`.
- `AX_SPLIT_3.OUT2` → `GreenWhiteBackground_AX.DI1`: `GreenWhiteBackground_AX` ([`MyLib::sys::GreenWhiteBackground3_AX`](GreenWhiteBackground3_AX.md)) colours the background green (TRUE) or white (FALSE); `u16ObjId` and `u16ObjIdA` go to its inputs of the same name.
- `AX_SPLIT_3.OUT3` → `PUBLISH_STATE.IN`: `PUBLISH_STATE` (`adapter::net::AX_PUBLISH_1`, `QI = TRUE`) publishes the state to the address `ID_STATE_WRITE` (`ID_STATE_WRITE` → `PUBLISH_STATE.ID`).

## Technical Details

- Extracted from `Baender_EinAus` (wired three times identically there for hedgehog belt, elevator and cross belt)
- `ID_STATE_WRITE` is new because the web mirror needs its own state publish for the individual belt softkeys
- There are no own event or data ports for the state; everything runs via the AX adapter `IN`

## Application Scenarios

- Group signal or OR-merged state that is to switch an output, show the softkey colour and be published for the web
- Several time-staggered channels, each supplying its own state

## Conclusion

`AX_TO_QXA_BG_PUBLISH` distributes a finished AX state to the physical output, background colour and state publish.
