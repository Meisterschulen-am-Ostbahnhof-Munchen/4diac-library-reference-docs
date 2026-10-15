# Softkey_Aux_Switch_Auto_TO_Remote_WRITE

![Softkey_Aux_Switch_Auto_TO_Remote_WRITE_network](./Softkey_Aux_Switch_Auto_TO_Remote_WRITE_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Switch_Auto_TO_Remote_WRITE` combines, by OR, five sources of a command: the VT SoftKey, the AUX assignment (joystick), a local web override, an additional physical button on another module (remote subscribe) and an autonomous automation command (`AUTO_IN`). The result is written to a target module via remote OPC-UA write (generic, one channel, without status feedback). The block is the 5-source extension of `Softkey_Aux_Switch_TO_Remote_WRITE` by the additional socket `AUTO_IN` (e.g. automatic filling hysteresis driving the same filling belt raise line as SoftKey/AUX/web/physical button).

![Softkey_Aux_Switch_Auto_TO_Remote_WRITE](Softkey_Aux_Switch_Auto_TO_Remote_WRITE.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirm

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, TRUE enables the service
- **u16ObjId** (UINT): object ID of the SoftKey (VT), default `ID_NULL`
- **u16ObjIdA** (UINT): object ID of the AuxFunction2 (VT), default `ID_NULL`
- **ID_WRITE_REMOTE** (WSTRING): remote write address to the target module (command, `ACTION=WRITE`, CLIENT)
- **ID_WEB_READ** (WSTRING): local subscribe address for a web client (e.g. vt-ui-mirror) on the same module, ORed with SoftKey and AUX
- **ID_SWITCH_REMOTE** (WSTRING): remote subscribe address of an additional physical button on another module, ORed with SoftKey/AUX/web/automation

### **Data Outputs**

- **QO** (`BOOL`): quality output, AND combination (`AND_QO`, `AND_BOOL_5`) of the `QO` outputs of `Softkey_IXA`, `Aux_IXA`, `SUBSCRIBE_WEB`, `SUBSCRIBE_SWITCH` and `AX_CLIENT_1_0`

### **Adapters**

- **AUTO_IN** (socket, `AX`): autonomous command (e.g. automatic filling hysteresis), ORed with SoftKey/AUX/web/physical button
- **AUTOMATIC_OUT** (plug, `AX`): optional; locally merged raw command (SoftKey OR AUX OR web OR switch OR automation) before the remote write. Leave unwired if not needed
- **MANUAL_OUT** (plug, `AX`): optional; only the manual sources (SoftKey OR AUX OR web OR switch), **without** `AUTO_IN`. This allows, for example, an automation to be switched off when the operator intervenes manually, without the automation switching itself off through its own command. Leave unwired if not needed

## Functionality

The network works in two OR stages:

1. **Manual sources**: `Softkey_IXA` (`isobus::UT::io::Softkey::Softkey_IXA`, `u16ObjId`) → `OR_Manual.IN1`, `Aux_IXA` (`isobus::UT::io::Auxiliary::IN::Aux_IXA`, `u16ObjIdA`) → `OR_Manual.IN2`, `SUBSCRIBE_WEB` (`AX_SUBSCRIBE_1`, address `ID_WEB_READ`) → `OR_Manual.IN3`, `SUBSCRIBE_SWITCH` (`AX_SUBSCRIBE_1`, address `ID_SWITCH_REMOTE`) → `OR_Manual.IN4`. `OR_Manual` is an `AX_OR_4`.
2. `OR_Manual.OUT` → `AX_SPLIT_2_Manual`: `OUT1` → `OR_Automatik.IN1`, `OUT2` → `MANUAL_OUT`.
3. **Automation**: `AUTO_IN` → `OR_Automatik.IN2`. `OR_Automatik` is an `AX_OR_2`.
4. `OR_Automatik.OUT` → `AX_SPLIT_2_CmdOut`: `OUT1` → `AUTOMATIC_OUT`, `OUT2` → `AX_CLIENT_1_0.IN`.
5. `AX_CLIENT_1_0` (`adapter::net::AX_CLIENT_1_0`) writes the command via OPC-UA to the address `ID_WRITE_REMOTE`.
6. `AND_QO` (`iec61131::booleanOperators::AND_BOOL_5`) combines the `QO` outputs of `Softkey_IXA` (`IN1`), `Aux_IXA` (`IN2`), `SUBSCRIBE_WEB` (`IN3`), `SUBSCRIBE_SWITCH` (`IN4`) and `AX_CLIENT_1_0` (`IN5`) into `QO`.
7. Initialization: `INIT` → `Softkey_IXA.INIT` → `Aux_IXA.INIT` → `SUBSCRIBE_WEB.INIT` → `SUBSCRIBE_SWITCH.INIT` → `AX_CLIENT_1_0.INIT`; `AX_CLIENT_1_0.INITO` → `INITO` and → `AND_QO.REQ`. `QI` is distributed to all five blocks.

## Technical Details

- The former `AX_OR_5` was split into `OR_Manual` (`AX_OR_4`) and `OR_Automatik` (`AX_OR_2`) so that `MANUAL_OUT` is available without the automation part; the overall behaviour for `AUTOMATIC_OUT` and the remote write remained unchanged
- `MANUAL_OUT` prevents self-shutdown: if the automation used its own command for switching off, it would switch itself off through it
- Every adapter is branched only through `AX_SPLIT_2`

## Application Scenarios

- Filling belt raise: the command can come from the operator (SoftKey/AUX/web/physical button S08) or from the automatic filling hysteresis and is written to the target module
- Automation that switches off as soon as the operator intervenes manually (via `MANUAL_OUT`)

## Conclusion

`Softkey_Aux_Switch_Auto_TO_Remote_WRITE` merges five command sources by OR, writes the result to a target module via OPC-UA and optionally provides two intermediate states via `AUTOMATIC_OUT` and `MANUAL_OUT`.
