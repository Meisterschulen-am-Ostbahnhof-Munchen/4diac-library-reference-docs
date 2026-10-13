# AX_SoftkeyToggle_RS_TO_AX

![AX_SoftkeyToggle_RS_TO_AX_network](./AX_SoftkeyToggle_RS_TO_AX_network.svg)

* * * * * * * * * *

## Introduction

`AX_SoftkeyToggle_RS_TO_AX` generates a pure AX state from a toggle trigger and a reset trigger. The toggle trigger comes from SoftKey, AUX, a local web/dashboard method or an IO diagnostics test button (via `Softkey_Aux_Method_Button_IE`), the reset trigger from SoftKey, AUX or a local method (via `Softkey_Aux_Method_IE`). Both drive an `AX_T_FF_SR` whose state is present at the plug `OUT`. There is **no** physical output, **no** background colour and **no** state publish; these are left to the caller.

![AX_SoftkeyToggle_RS_TO_AX](AX_SoftkeyToggle_RS_TO_AX.svg)

## Interface Structure

### **Event Inputs**

- **AUS**: hard-resets the state to OFF from outside as an event (e.g. a centrally read common SoftKey 'All off'); in addition to the block's own reset path via `u16ObjId_RESET`/`u16ObjIdA_RESET`/`ID_RESET_METHOD`. Leave unwired if not needed.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

- **u16ObjId** (UINT): object ID of the SoftKey on/off (toggle), default `ID_NULL`
- **u16ObjIdA** (UINT): object ID of the AuxFunction2 (joystick) for the toggle, equal in rank to the SoftKey
- **u16ObjIdB** (UINT): object ID of the IO diagnostics test button (outputs diagnostics page), toggles with equal rank; leave `ID_NULL` if no such button is involved
- **u16ObjId_RESET** (UINT): object ID of the SoftKey 'All off' (reset, hard to OFF)
- **u16ObjIdA_RESET** (UINT): object ID of the AuxFunction2 (joystick) for the reset, equal in rank to the SoftKey 'All off'
- **ID_TOGGLE_METHOD** (WSTRING): local method address (`ACTION=CREATE_METHOD`) for the argument-less toggle trigger; called by the web client (vt-ui-mirror) and directly by the OPC dashboard
- **ID_RESET_METHOD** (WSTRING): local method address (`ACTION=CREATE_METHOD`) for the argument-less reset trigger (hard to OFF); called by the web client and the OPC dashboard

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **OUT** (plug, `adapter::types::unidirectional::AX`): toggle state (`AX_T_FF_SR.Q`); further processing (output, background, delay) is left to the caller
- **FORCE_OFF** (socket, `adapter::types::unidirectional::AX`): optional; a rising edge hard-resets to OFF (e.g. safety shutdown). Leave unwired or at a fixed FALSE if not needed

## Functionality

The network contains four parts:

- `Toggle` (`MyLib::sys::Softkey_Aux_Method_Button_IE`): reads `u16ObjId`, `u16ObjIdA`, `u16ObjIdB` and `ID_TOGGLE_METHOD` (to `Toggle.ID_METHOD`); its `IND` goes to `AX_T_FF_SR.CLK` and toggles the state.
- `Reset` (`MyLib::sys::Softkey_Aux_Method_IE`): reads `u16ObjId_RESET` (to `Reset.u16ObjId`), `u16ObjIdA_RESET` (to `Reset.u16ObjIdA`) and `ID_RESET_METHOD`; its `IND` goes to `AX_T_FF_SR.R`.
- `AX_T_FF_SR` (`adapter::events::unidirectional::AX_T_FF_SR`): toggle flip-flop with set/reset; `AX_T_FF_SR.Q` → `OUT`.
- `AX_R_TRIG_ForceOff` (`adapter::events::unidirectional::AX_R_TRIG`): detects the rising edge on `FORCE_OFF` (`FORCE_OFF` → `AX_R_TRIG_ForceOff.QI`); its `EO` also goes to `AX_T_FF_SR.R`.

The event `AUS` is a third, additive path to `AX_T_FF_SR.R`.

## Technical Details

- Base block: for **one** output there is `MyLib::sys::AX_SoftkeyToggle_RS_TO_QXA_BG`, which wraps this block. For several time-staggered outputs (e.g. belt sequencing) this block is used directly and the `OUT` state is processed per channel with `AX_TON`/`AX_TOF`
- Three equal reset paths (VT/AUX/method, event `AUS`, edge on `FORCE_OFF`) all end at `AX_T_FF_SR.R`
- The reset input is called `AUS`, matching the central 'All off' softkey that several toggle instances distribute together

## Application Scenarios

- On/off switch for drives operated equally by SoftKey, joystick assignment, web/dashboard and diagnostics button
- Central 'All off' for several toggle instances via the event `AUS`
- Safety shutdown via `FORCE_OFF` (e.g. PTO monitoring)

## Conclusion

`AX_SoftkeyToggle_RS_TO_AX` supplies the pure toggle state with several equal operating and reset paths and leaves output, background colour and publish to the caller.
