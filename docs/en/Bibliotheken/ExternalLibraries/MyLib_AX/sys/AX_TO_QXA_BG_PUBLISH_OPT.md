# AX_TO_QXA_BG_PUBLISH_OPT

![AX_TO_QXA_BG_PUBLISH_OPT_network](./AX_TO_QXA_BG_PUBLISH_OPT_network.svg)

* * * * * * * * * *

## Introduction

`AX_TO_QXA_BG_PUBLISH_OPT` corresponds to [`AX_TO_QXA_BG_PUBLISH`](./AX_TO_QXA_BG_PUBLISH.md), but gates the physical output separately: background and state publish always run via `QI`/`INIT`, only the `DigitalOutput` depends on `QI_Output`/`INIT_Output` (e.g. option "classic" versus "PVEA" on a shared pin). The block is a copy of `AX_TO_QXA_BG_PUBLISH` v1.2; the reason was that with the PVEA variant the SoftKey background and state publish stayed inactive together with the output.

![AX_TO_QXA_BG_PUBLISH_OPT](AX_TO_QXA_BG_PUBLISH_OPT.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization of background and publish (display); typically right at boot
- **INIT_Output** (`EInit`): initialization of the physical output; typically driven by the option

### **Event Outputs**

- **INITO** (`EInit`): display initialized; real confirm from `PUBLISH_STATE.INITO`
- **INITO_Output** (`EInit`): output initialized; real confirm from `DigitalOutput.INITO`

### **Data Inputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID SoftKey; carries the background colour. |
| `u16ObjIdA` | UINT | `ID_NULL` | Object ID AuxFunction2 (joystick); also shows the background colour on the Aux handle. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Selection of the output `Output_Q1`..`Output_Q12`. |
| `ID_STATE_WRITE` | WSTRING |  | Local publish address (`ACTION=WRITE`) for the state; subscribed remotely by web client/dashboard. |
| `QI` | BOOL | TRUE | Qualifier for background and publish (display). |
| `QI_Output` | BOOL | TRUE | Qualifier for the physical output only; FALSE releases the pin. |

### **Data Outputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `QO` | BOOL |  | Quality output display (AND of `GreenWhiteBackground_AX.QO` and `PUBLISH_STATE.QO`). |
| `QO_Output` | BOOL |  | Quality output physical output (`DigitalOutput.QO`). |

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `IN` | `adapter::types::unidirectional::AX` | Socket | Finished AX state; distributed to output, background and publish. |

## Function Blocks (FBs) Used

- **AX_SPLIT_3** (`adapter::events::unidirectional::AX_SPLIT_3`): adapter fan-out (strictly 1:1)
- **DigitalOutput** (`logiBUS::io::DQ::logiBUS_QXA`): physical output
- **GreenWhiteBackground_AX** (SubApp `MyLib::sys::GreenWhiteBackground3_AX`): background colour of SoftKey and Aux; see [`GreenWhiteBackground3_AX`](./GreenWhiteBackground3_AX.md)
- **PUBLISH_STATE** (`adapter::net::AX_PUBLISH_1`): local state publish
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): combines the `QO` of the display

## Program Flow and Connections

1. AX state: `IN` -> `AX_SPLIT_3.IN`; `OUT1` -> `DigitalOutput.OUT`; `OUT2` -> `GreenWhiteBackground_AX.DI1`; `OUT3` -> `PUBLISH_STATE.IN`.
2. Output initialization: `INIT_Output` -> `DigitalOutput.INIT`; `DigitalOutput.INITO` -> `INITO_Output`.
3. Display initialization: `INIT` -> `GreenWhiteBackground_AX.INIT`; `GreenWhiteBackground_AX.INITO` -> `PUBLISH_STATE.INIT`; `PUBLISH_STATE.INITO` -> `INITO` and -> `AND_QO.REQ`.
4. Qualifier: `QI` -> `GreenWhiteBackground_AX.QI` and `PUBLISH_STATE.QI`; `QI_Output` -> `DigitalOutput.QI`.
5. Quality: `GreenWhiteBackground_AX.QO` -> `AND_QO.IN1`; `PUBLISH_STATE.QO` -> `AND_QO.IN2`; `AND_QO.OUT` -> `QO`; `DigitalOutput.QO` -> `QO_Output`.
6. Data: `u16ObjId`, `u16ObjIdA` -> `GreenWhiteBackground_AX`; `Output` -> `DigitalOutput.Output`; `ID_STATE_WRITE` -> `PUBLISH_STATE.ID`.

## Technical Features

- `QI`/`INIT` now apply only to background and publish; `QI_Output`/`INIT_Output`/`INITO_Output`/`QO_Output` belong exclusively to the `DigitalOutput`.
- The `AX_SPLIT_3` is needed because the adapter fan-out is strictly 1:1.

## Application Scenarios

- Channels where the physical output can be switched off via an option (e.g. classic versus PVEA on a shared pin), while SoftKey background and state publish should always run.

## Summary

`AX_TO_QXA_BG_PUBLISH_OPT` distributes a finished AX state to output, background and publish, with the physical output enabled and initialized separately.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
