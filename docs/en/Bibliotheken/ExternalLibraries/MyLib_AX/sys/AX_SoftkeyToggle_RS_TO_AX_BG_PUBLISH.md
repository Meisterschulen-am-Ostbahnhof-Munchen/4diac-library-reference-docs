# AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH

![AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH_network](./AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH_network.svg)

* * * * * * * * * *

## Introduction

`AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH` wraps [`AX_SoftkeyToggle_RS_TO_AX`](./AX_SoftkeyToggle_RS_TO_AX.md) (toggle/reset state) and drives the background colour and a local state publish from it, **without** a physical output. The toggle is triggered by SoftKey, AUX, local web/dashboard method or IO-diag test button; the reset by SoftKey, AUX or method. The background colour (SoftKey and AUX) comes from one shared `GreenWhiteBackground3_AX`. The block is the counterpart of `AX_SoftkeyToggle_RS_TO_QXA_BG`, which additionally drives a `logiBUS_QXA`. It is meant for cases in which the caller processes the toggle state further before it goes to one or more physical outputs (e.g. conveyor sequencing: key 5 toggles, only then `AX_TON`/`AX_TOF` per channel). The toggle state remains available as plug `OUT`.

![AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH](AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH.svg)

## Interface Structure

### **Event Inputs**

- **AUS**: hard reset to OFF, from outside as an event (e.g. one centrally read shared SoftKey). Pass-through to `ToggleState.AUS`. Leave unwired if not needed.

### **Event Outputs**

None.

### **Data Inputs**

- **u16ObjId** (`UINT`): object ID of the on/off softkey (toggle), also carries the background colour (default `ID_NULL`)
- **u16ObjIdA** (`UINT`): object ID of the AuxFunction2 (joystick) for the toggle, equal in rank to the SoftKey, also shows the background colour on the aux handle (default `ID_NULL`)
- **u16ObjIdB** (`UINT`): object ID of the IO-diag test button (outputs diagnostic page), toggles with equal rank; no background colour of its own because `GreenWhiteBackground3_AX` only knows SoftKey and AUX (default `ID_NULL`)
- **u16ObjId_RESET** (`UINT`): object ID of the "all off" softkey (reset, hard to OFF), e.g. `SoftKey_AUS` (default `ID_NULL`)
- **u16ObjIdA_RESET** (`UINT`): object ID of the AuxFunction2 (joystick) for the reset, equal in rank to the "all off" softkey (default `ID_NULL`)
- **ID_TOGGLE_METHOD** (`WSTRING`): local method address (`ACTION=CREATE_METHOD`) for the argument-less toggle trigger, called by the web client (vt-ui-mirror) and directly by the OPC dashboard
- **ID_RESET_METHOD** (`WSTRING`): local method address (`ACTION=CREATE_METHOD`) for the argument-less reset trigger (hard to OFF), called by the web client and the OPC dashboard
- **ID_STATE_WRITE** (`WSTRING`): local publish address (`ACTION=WRITE`) for the actual toggle state (`AX_T_FF_SR.Q`), subscribed remotely by web client/dashboard

### **Data Outputs**

None.

### **Adapters**

- **FORCE_OFF** (socket, `adapter::types::unidirectional::AX`): optional; a rising edge hard-resets to OFF (e.g. safety shutdown). Pure pass-through socket to `ToggleState.FORCE_OFF`. Leave unwired (fixed FALSE literal) if not needed.
- **OUT** (plug, `adapter::types::unidirectional::AX`): toggle state (`AX_T_FF_SR.Q`) for further processing by the caller; background and publish are already handled here

## Function Blocks (FBs) Used

- **`ToggleState`** (SubApp `MyLib::sys::AX_SoftkeyToggle_RS_TO_AX`): generates the toggle/reset state
- **`AX_SPLIT_3`** (`adapter::events::unidirectional::AX_SPLIT_3`): distributes the state to three targets
- **`GreenWhiteBackground_AX`** (SubApp `MyLib::sys::GreenWhiteBackground3_AX`): green/white background colour
- **`PUBLISH_STATE`** (`adapter::net::AX_PUBLISH_1`, `QI = TRUE`): publishes the state

## Functionality

- `AUS` → `ToggleState.AUS`; `FORCE_OFF` → `ToggleState.FORCE_OFF`
- `u16ObjId`, `u16ObjIdA`, `u16ObjIdB`, `u16ObjId_RESET`, `u16ObjIdA_RESET`, `ID_TOGGLE_METHOD`, `ID_RESET_METHOD` → the like-named inputs of `ToggleState`
- `ToggleState.OUT` → `AX_SPLIT_3.IN`
- `AX_SPLIT_3.OUT1` → `OUT`
- `AX_SPLIT_3.OUT2` → `GreenWhiteBackground_AX.DI1`; `u16ObjId`, `u16ObjIdA` → `GreenWhiteBackground_AX`
- `AX_SPLIT_3.OUT3` → `PUBLISH_STATE.IN`; `ID_STATE_WRITE` → `PUBLISH_STATE.ID`

## Technical Details

- Extracted from `Baender_EinAus` (ToggleState/`AX_SPLIT_3` part for background and publish, `GreenWhiteBackground_AX`, `PUBLISH_STATE`). The time-staggered outputs (`TOF_IgelElevator`/`TON_Querband` staggering) stay with the caller.
- Version history: `FORCE_OFF` (v1.1, for a higher-level safety shutdown), `RESET` pass-through (v1.2), input `RESET` renamed to `AUS` (v1.3).
- Imports `isobus::UT::Q::const::IDs::ID_NULL` as default for the object IDs.

## Application Scenarios

- On/off toggle with colour indication and web publish whose state is processed further before the physical output (e.g. staggered conveyors)

## Summary

`AX_SoftkeyToggle_RS_TO_AX_BG_PUBLISH` provides a toggle/reset state with background colour and state publish and outputs the state as an `AX` plug for further processing.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
