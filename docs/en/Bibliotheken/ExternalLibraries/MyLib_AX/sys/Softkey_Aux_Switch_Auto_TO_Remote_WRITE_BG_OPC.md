# Softkey_Aux_Switch_Auto_TO_Remote_WRITE_BG_OPC

![Softkey_Aux_Switch_Auto_TO_Remote_WRITE_BG_OPC_network](./Softkey_Aux_Switch_Auto_TO_Remote_WRITE_BG_OPC_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_Switch_Auto_TO_Remote_WRITE_BG_OPC` combines, for a command to a target module: reading the VT **SoftKey** OR reading the **AUX assignment** (joystick) OR a local **web override** OR a physical **remote switch** on a third module OR an **automatic command**, plus a **remote OPC-UA write** to the target module and the **VT background colour** (SoftKey and AUX) from a remote subscribe. It is the 5-source variant of [`Softkey_Aux_Switch_TO_Remote_WRITE_BG_OPC`](./Softkey_Aux_Switch_TO_Remote_WRITE_BG_OPC.md) with an additional `AUTO_IN` socket, e.g. filling conveyor raise: SoftKey/AUX/web/physical switch S08 AND automatic filling hysteresis.

![Softkey_Aux_Switch_Auto_TO_Remote_WRITE_BG_OPC](Softkey_Aux_Switch_Auto_TO_Remote_WRITE_BG_OPC.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

- **u16ObjId** (`UINT`): object ID of SoftKey/background (VT)
- **u16ObjIdA** (`UINT`): object ID of AuxFunction2/background (VT)
- **ID_SUBSCRIBE** (`WSTRING`): remote subscribe address (status/colour, `ACTION=SUBSCRIBE`)
- **ID_WRITE_REMOTE** (`WSTRING`): remote write address to the target module (command, `ACTION=WRITE`, CLIENT)
- **ID_WEB_READ** (`WSTRING`): local subscribe address for a web client (e.g. vt-ui-mirror) on the same module, OR-ed with SoftKey and AUX
- **ID_SWITCH_REMOTE** (`WSTRING`): remote subscribe address of an additional physical switch on another module, OR-ed with SoftKey/AUX/web/automatic
- **ID_STATUS_WEB** (`WSTRING`): local publish address (on this module) for the state reported back by the target module, so that a web client can show the same background colour as the real VT

### **Data Outputs**

None.

### **Adapters**

All adapters are of type `adapter::types::unidirectional::AX`.

- **AUTO_IN** (socket): autonomous command (e.g. automatic filling hysteresis), OR-ed with SoftKey/Aux/web/physical switch
- **AUTOMATIC_OUT** (plug): optional, locally merged raw command, passed through from `Command.AUTOMATIC_OUT`; leave unwired if not needed
- **MANUAL_OUT** (plug): optional, only the manual sources (SoftKey/AUX/web/switch) without the automatic part, passed through from `Command.MANUAL_OUT`; leave unwired if not needed

## Function Blocks (FBs) Used

- **`Command`** (SubApp `MyLib::sys::Softkey_Aux_Switch_Auto_TO_Remote_WRITE`, [Softkey_Aux_Switch_Auto_TO_Remote_WRITE](./Softkey_Aux_Switch_Auto_TO_Remote_WRITE.md)): forms the command from all sources and writes it remotely
- **`Status`** (SubApp `MyLib::sys::AX_SUBSCRIBE_BG3_WEB_OPC`, [AX_SUBSCRIBE_BG3_WEB_OPC](./AX_SUBSCRIBE_BG3_WEB_OPC.md)): subscribes to the state, colours the background and publishes it for the web client

## Functionality

- `u16ObjId`, `u16ObjIdA`, `ID_WRITE_REMOTE`, `ID_WEB_READ`, `ID_SWITCH_REMOTE` → the like-named inputs of `Command`
- `AUTO_IN` → `Command.AUTO_IN`
- `Command.AUTOMATIC_OUT` → `AUTOMATIC_OUT`; `Command.MANUAL_OUT` → `MANUAL_OUT`
- `u16ObjId`, `u16ObjIdA`, `ID_SUBSCRIBE`, `ID_STATUS_WEB` → the like-named inputs of `Status`

## Technical Details

- Command path (`Command`) and status path (`Status`) are separate: the command goes to the target module via remote write, the background colour comes from the state reported back by the target module
- Optional outputs `AUTOMATIC_OUT`/`MANUAL_OUT` allow local reuse of the merged command or of the manual part only

## Application Scenarios

- A mixed manual/automatic operating point, e.g. filling conveyor raise (SoftKey, AUX, web, physical switch S08 and automatic filling)

## Summary

The SubApp bundles five command sources, a remote write to the target module and the reported VT background colour.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
