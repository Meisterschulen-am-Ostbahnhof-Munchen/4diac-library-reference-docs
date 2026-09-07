# AX_SUBSCRIBE_BG3_WEB_OPC

![AX_SUBSCRIBE_BG3_WEB_OPC_network](./AX_SUBSCRIBE_BG3_WEB_OPC_network.svg)

* * * * * * * * * *

## Introduction

`AX_SUBSCRIBE_BG3_WEB_OPC` is the status half of `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`, extracted as an independent component: a remote subscribe value is displayed as a background color on both a SoftKey AND an AuxFunction2 (`GreenWhiteBackground3_AX`) and is also republished locally via OPC UA so that a web client (e.g., vt-ui-mirror) on the same module can read the same status without needing its own connection to the target module.


## Function Blocks (FBs) Used

### Sub-Blocks: AX_SUBSCRIBE_BG3_WEB_OPC

- **Type**: SubAppType
- **Internal FBs Used**:

- **AX_SUBSCRIBE_1**: `adapter::net::AX_SUBSCRIBE_1` (`QI=TRUE`) — Remote subscription of the status/color (`ID_SUBSCRIBE`).

- **AX_SPLIT_2**: `adapter::events::unidirectional::AX_SPLIT_2` — Branches the received signal to the VT display and the local web republish.

- **GreenWhiteBackground3_AX** (SubApp, `MyLib::sys`): Synchronously colors both a SoftKey (`u16ObjId`) and an AuxFunction2 (`u16ObjIdA`).

- **STATUS_WEB_PUBLISH**: `adapter::net::AX_PUBLISH_1` (`QI=TRUE`) — Republishes the received state to this module's local OPC UA server (`ID_STATUS_WEB`).

- **How it works**: The state received via remote subscribe is simultaneously passed to the VT background color and the local web republish server via `AX_SPLIT_2`.


## Program Flow and Connections

1. `u16ObjId` → `GreenWhiteBackground3_AX.u16ObjId`; `u16ObjIdA` → `GreenWhiteBackground3_AX.u16ObjIdA`; `ID_SUBSCRIBE` → `AX_SUBSCRIBE_1.ID`; `ID_STATUS_WEB` → `STATUS_WEB_PUBLISH.ID` (Data connections, hidden).

2. `AX_SUBSCRIBE_1.OUT` → `AX_SPLIT_2.IN` → `AX_SPLIT_2.OUT1` → `GreenWhiteBackground3_AX.DI1` (VT display), `AX_SPLIT_2.OUT2` → `STATUS_WEB_PUBLISH.IN` (Web Republish).

## Technical Features

- **Extension of `AX_SUBSCRIBE_BG_OPC`**: Compared to the simpler version (only Subscribe + background color), two things are added: `GreenWhiteBackground3_AX` instead of `GreenWhiteBackground1_AX` (colors SoftKey and Aux together) and local Web Republish via `AX_SPLIT_2`/`STATUS_WEB_PUBLISH`.
... - **Decoupling from the target module**: A web client connected only to this module does not need to establish its own connection to the target module to see the same status as the actual VT.

## Application Scenarios

- Status display for functions that need to be displayed synchronously on both a SoftKey and an AuxFunction2 (joystick assignment), plus web mirroring.

- For a SoftKey without an Aux, use `AX_SUBSCRIBE_BG_OPC` instead (no web republish).

## Comparison with similar modules

For the complete, pre-wired combination with the command-line interface (SoftKey/Aux read + remote write), continue to use `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`, which includes this module as a status sub-module.


## Summary

`AX_SUBSCRIBE_BG3_WEB_OPC` encapsulates the status page of a remote channel: reception, synchronous softkey/aux background color, and local web republish in a self-contained, reusable building block.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de ](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
