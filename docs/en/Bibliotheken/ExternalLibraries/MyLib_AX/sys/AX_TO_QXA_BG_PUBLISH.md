# AX_TO_QXA_BG_PUBLISH

![AX_TO_QXA_BG_PUBLISH_network](./AX_TO_QXA_BG_PUBLISH_network.svg)

* * * * * * * * * *

## Introduction

`AX_TO_QXA_BG_PUBLISH` drives, from an already finished AX state (e.g. merged by OR of several sources such as a staggered group signal, single toggle or IO diagnostics test button), **one** physical output, the background colour (SoftKey and AUX) and a local state publish for web client and OPC dashboard. It is the counterpart to `MyLib::sys::AX_SoftkeyToggle_RS_TO_QXA_BG`, which still generates the toggle/reset itself; here the AX state is already available.

![AX_TO_QXA_BG_PUBLISH](AX_TO_QXA_BG_PUBLISH.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialisation; passed on to `DigitalOutput.INIT` instead of automatic distribution

### **Event Outputs**

- **INITO** (`EInit`): initialisation completed; real confirm from `PUBLISH_STATE.INITO` at the end of the chain `DigitalOutput` → `GreenWhiteBackground_AX` → `PUBLISH_STATE`

### **Data Inputs**

- **QI** (BOOL): qualifier; goes to `DigitalOutput`, `GreenWhiteBackground_AX` and `PUBLISH_STATE`, FALSE keeps this instance completely inactive (default `TRUE`)
- **u16ObjId** (UINT): object ID of the SoftKey, carries the background colour (default `ID_NULL`)
- **u16ObjIdA** (UINT): object ID of the AuxFunction2 (joystick), also shows the background colour at the aux handle (default `ID_NULL`)
- **Output** (`logiBUS::io::DQ::logiBUS_DO_S`): selection of the output `Output_Q1`..`Output_Q12` (default `logiBUS_DO::Invalid`)
- **ID_STATE_WRITE** (WSTRING): local publish address (`ACTION=WRITE`) for the actual state; subscribed remotely by web client/dashboard

### **Data Outputs**

- **QO** (BOOL): quality output - TRUE if the service was executed successfully (AND of `DigitalOutput`, `GreenWhiteBackground_AX` and `PUBLISH_STATE`)

### **Adapters**

- **IN** (socket, `adapter::types::unidirectional::AX`): finished, already merged AX state; distributed to output, background and publish

## Functionality

`IN` is routed to an `AX_SPLIT_3` (`adapter::events::unidirectional::AX_SPLIT_3`), which distributes the state to three destinations:

- `AX_SPLIT_3.OUT1` → `DigitalOutput.OUT`: `DigitalOutput` (`logiBUS::io::DQ::logiBUS_QXA`) switches the physical output; `Output` → `DigitalOutput.Output`, `QI` → `DigitalOutput.QI` and the event `INIT` → `DigitalOutput.INIT`.
- `AX_SPLIT_3.OUT2` → `GreenWhiteBackground_AX.DI1`: `GreenWhiteBackground_AX` ([`MyLib::sys::GreenWhiteBackground3_AX`](GreenWhiteBackground3_AX.md)) colours the background green (TRUE) or white (FALSE); `u16ObjId` and `u16ObjIdA` go to its inputs of the same name, `QI` to `GreenWhiteBackground_AX.QI`.
- `AX_SPLIT_3.OUT3` → `PUBLISH_STATE.IN`: `PUBLISH_STATE` (`adapter::net::AX_PUBLISH_1`, `QI` from outside) publishes the state to the address `ID_STATE_WRITE` (`ID_STATE_WRITE` → `PUBLISH_STATE.ID`).

Initialisation and quality: `INIT` → `DigitalOutput.INIT`, `DigitalOutput.INITO` → `GreenWhiteBackground_AX.INIT`, `GreenWhiteBackground_AX.INITO` → `PUBLISH_STATE.INIT`, `PUBLISH_STATE.INITO` → `INITO`. `QO` is the output of the instance `AND_QO` (`iec61131::booleanOperators::AND_BOOL_3`) with `DigitalOutput.QO`, `GreenWhiteBackground_AX.QO` and `PUBLISH_STATE.QO` at `IN1`..`IN3`; its `REQ` is triggered by `PUBLISH_STATE.INITO`.

## Technical Details

- Extracted from `Baender_EinAus` (wired three times identically there for hedgehog belt, elevator and cross belt)
- `ID_STATE_WRITE` is new because the web mirror needs its own state publish for the individual belt softkeys
- Version 1.1: `QI` and `INIT` are passed on to `DigitalOutput` (instead of a hard `TRUE` and automatic distribution), so that e.g. classic-vs-PVEA variants sharing pins can keep an instance inactive and the initialisation can be sequenced by the caller
- Version 1.2: new event output `INITO` (initialisation completed) and data output `QO` (AND of the inner `QO`); initialisation runs as a chain `DigitalOutput` → `GreenWhiteBackground_AX` → `PUBLISH_STATE`, `QI` is distributed to all three instances
- There are no own event or data ports for the state; everything runs via the AX adapter `IN`

## Application Scenarios

- Group signal or OR-merged state that is to switch an output, show the softkey colour and be published for the web
- Several time-staggered channels, each supplying its own state

## Conclusion

`AX_TO_QXA_BG_PUBLISH` distributes a finished AX state to the physical output, background colour and state publish.
